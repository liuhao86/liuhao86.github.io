---
layout: post
title:  "SpringBoot静态资源404的一种原因"
date:  2018-05-30 17:06:45 +0800
categories:
  - SpringBoot
tag:
  - springboot

---

* content
{:toc}


### 问题说明
1. 静态资源放置到规定的位置, 不管是war包内部还是外部一律404
2. 调整`spring.resource.static-resoure-locations`依旧没有效果

### 问题解决
 启动类上使用了`@EnableMvc`,删除即解决

### 问题分析
1. `@EnableMvc`会导致加载Spring加载Bean `DelegatingWebMvcConfiguration`.
2. WebMvcAutoConfiguration的有一个注解声明如下:`@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)`
3. 当`WebMvcConfigurationSupport` Bean初始化,`WebMvcAutoConfiguration`放弃初始化
4. 而我们需要`WebMvcAutoConfiguration`的`addResourceHandlers`方法

### 问题延伸
1. 使用`@EnableMvc`的时候需要声明`MultipartResolver`,删除之后则不需要, 否则可能导致上传文件流被两次读取,导致获取文件失败
