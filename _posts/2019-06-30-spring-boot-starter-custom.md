---
layout: post
title:  "开发自己的Spring Boot Starter"
date:  2019-06-30 21:21:04 +0800
categories:
  - SpringBoot
tag:
  - SpringBoot

---

* content
{:toc}

## 背景
按照最近Jetbrains发布的开发者生态系统现状（https://www.jetbrains.com/zh-cn/lp/devecosystem-2019/），

> Spring Boot 已成为最流行的 Java web 框架，自去年以来增加 14%。
> ![Spring Boot Diagram 1](/resources/7.png)

## 好处
统一是它最大的优势，一套经过考验的的使用方式，而且使用起来非常简单、易用、灵活。
有很好的生态，常用的类库的都提供了相应的Spring Boot Starter包，比如Mybatis, Druid, Dubbo

## 动手

### 推荐方式
关于创建自己的Spring Boot Starter的官方文档： https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-developing-auto-configuration.html

官网推荐的Spring Boot Stater开发方式，需要创建两个项目：
1. autoConfigure，包含自动注册相关,依赖下面的Starter，(可以多个项目共用这一个autoconfigure项目,具体可以参考Spring的实现： spring-boot-autoconfigure, 包含spring boot官方提供的所有starter的AutoConfiguration)
2. starter, 包含所有的业务逻辑和依赖

autoconfigure最重要的是用好@ConditionOn*这一批注解，一个Bean什么情况下注入，依赖哪个配置，初始化顺序需要在autoconfigure中通过注解来声明，比如文件存储的例子：

项目结构如下：
```
|- spring-boot-starter-autoconfigure
|- spring-boot-starter-file-storage
|- spring-boot-starter-file-storage-oss
|- spring-boot-starter-file-storage-fastdfs
```

`spring-boot-starter-file-storage`声明文件存储的接口，其他两个项目分别包含OSS和FastDFS的实现，就可以在autoconfigure项目中这样声明：

``` java

    @Bean
    @ConditionalOnMissingBean
    @ConditionalOnClass(OSSClient.class)
    @ConditionalOnProperty("yonyoucloud.upesn.file.oss.bucket-name")
    public FileStorageService ossFileStorageService() {
        return new FileStorageServiceOSSImpl();
    }

    @Bean
    @ConditionalOnMissingBean
    @ConditionalOnClass(TrackerServer.class)
    @ConditionalOnProperty("yonyoucloud.upesn.file.fastdfs.host")
    public FileStorageService fastDfsFileStorageService() {
        return new FileStorageServiceFastDFSImpl();
    }

```
实际使用过程中，只要引入对应的实现类库，配置文件中增加响应的配置，就可以使用自动注入Bean的方式使用；

甚至可以同时引入OSS和FastDFS两个库，仅通过是否配置对应的属性来控制使用哪一个，在多个不同部署的环境中共用一份代码或者部署文件时非常好用。
如果两种类库都引入了，两个需要的配置也都配置了，由于存在`@ConditionalOnMissingBean`，按照上面的方式，根据声明顺序会优先初始化OSS的实现，不会进行FastDFS的实现。

### 懒人模式
同时进行两个项目的开发，可能会觉得麻烦，没有autoconfigure项目，只开发一个项目也是没有问题的。

项目结构如下：
```
|- spring-boot-starter-file-storage
    └── src
        ├── main
        └── resources
           └── META-INF
               └── spring.factories
|- spring-boot-starter-file-storage-oss
|- spring-boot-starter-file-storage-fastdfs
```

不过AutoConfiguration里面的Bean的声明需要放到各自的实现中。

``` java

    /**
    * OSS项目中声明
    *
    */
    @Bean
    @ConditionalOnMissingBean
    @ConditionalOnClass(OSSClient.class)
    @ConditionalOnProperty("yonyoucloud.upesn.file.oss.bucket-name")
    public FileStorageService ossFileStorageService() {
        return new FileStorageServiceOSSImpl();
    }

    /**
    * FASTDFS项目中声明
    */
    @Bean
    @ConditionalOnMissingBean
    @ConditionalOnClass(TrackerServer.class)
    @ConditionalOnProperty("yonyoucloud.upesn.file.fastdfs.host")
    public FileStorageService fastDfsFileStorageService() {
        return new FileStorageServiceFastDFSImpl();
    }

```

使用方式相同，唯有在同时被引用又同时配置的情况下，哪一个被使用不确定，不过可以强制加上顺序，通过@AutoConfigureAfter和@Import这两个注解，如优先进行OSS初始化

``` java
@Slf4j
@RequiredArgsConstructor
@Configuration
@AutoConfigureAfter(OssFileStorageAutoConfigutaion.class)
@Import(OssFileStorageAutoConfigutaion.class)
@EnableConfigurationProperties(OssProperties.class)
public class FastDfsFileStorageAutoConfiguration {

    @Bean
    @ConditionalOnMissingBean
    @ConditionalOnClass(TrackerServer.class)
    @ConditionalOnProperty("yonyoucloud.upesn.file.oss.bucket-name")
    public FileStorageService fastDfsFileStorageService() {
        return new FileStorageServiceFastDfsImpl();
    }
```