---
layout: post
title:  "Maven Release Plguin实战"
date:  2017-04-13 14:22:36 +0800
categories:
  - Maven
tag:
  - maven
  - java

---

* content
{:toc}


### 简介
maven-release-plugin是一个自动将Maven项目的SNAPSHOT版本变更为RELEASE版本的工具插件.

不使用该插件时, 原有的步骤为:
1. 迁出新的分支或Tag
2. 更改所有依赖的SNAPSHOT为RELEASE
3. 更改自身的SNAPSHOT为RELEASE
4. 提交到SCM
5. 构建并部署到Nexus私服
6. 切换回develop分支
6. 在原SNAPSHOT基础上, 升级SNAPSHOT版本

### 优点
1. 自动替换pom文件中的SNAPSHOT依赖到release依赖
2. 自动Tag, 并且推送到远程SCM, 不需要新建本地分支
3. 自动上传Release到Nexus私服
4. 完成后自动升级SNAPSHOT版本号

### 要求
1. `mvn javadoc:javadoc`能够成功执行
2. 本地所有更改已经commit
3. pom文件中有scm的配置, 如下

``` xml
  <scm><connection>scm:git:https://github.com/liuhao86/liuhao86.github.io.git</connection></scm>
```
4. pom文件中所有依赖的SNAPSHOT有对应的release版本

### 步骤
1. 检查scm配置
2. 执行`mvn javadoc:javadoc`
3. 执行`mvn release:prepare`
4. 上一步成功后, 执行`mvn release:perform`

### 回滚/清理
执行失败, 需要更改会原来的pom, 并删除临时文件时, 执行`mvn release:rollback`和`mvn release:clean`, 若执行失败, 请手动删除临时文件, 并重命名pom.xml

### 注意事项

4. 多个项目相互依赖时, 可以先手动deploy 比较底层的 release版本, 否则没有找不到release版本, 无法编译成功
1. prepare时, 会生成`release.properties`文件, 请勿提交到GIT库, 已添加到默认.gitignore上, 注意更新
2. prepare时, 原来的pom.xml会变成`pom.xml.releaseBackup`
6. maven-release-plugin默认的release版本号为A.B.C, 不建议更改格式, 否则下个版本的SNAPSNOT每次都需要手动设定

### 常见问题
当执行执行`mvn release:perform`或者`mvn clean deploy`时出现transfer jar failed, 400, bad request时, 检查是否重复提交release版本.
> 一般不允许重复提交Release版本, 需要升级版本号再次提交, 如果需要强制提交, 需要在Nexus的`Release Repository`的`Configuration`中设置Deployment Pplicy为`Allow Redeploy`
