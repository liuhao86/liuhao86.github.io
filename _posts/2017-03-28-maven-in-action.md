---
layout: post
title:  "Maven知识点备忘"
date:   2017-03-28 16:16:40 +0800
categories:
  - Java
tag:
  - maven
  - java

---

* content
{:toc}

#### 依赖配置`optional`
适用于:
1. 解决循环依赖
2. 包A对另外一个包B是可选的, C依赖B时候并不一定需要A, 可以在B的依赖中配置A的`optional`为`true`

``` xml
<dependency>
  <groupId>sample.ProjectA</groupId>
  <artifactId>Project-A</artifactId>
  <version>1.0</version>
  <scope>compile</scope>
  <optional>true</optional> <!-- value will be true or false only -->
</dependency>
```
这是我比较喜欢的一种方式, 特别是一个包中仅很少情况下用户到的依赖, 通过`optional`控制范围, 就不会将自己依赖的一堆不重要的包, 再传递给依赖自己的项目中, 缩小引用的范围, 减少打出的可执行包的大小

对于`optional`, maven官网上有一句话:
>It may be helpful to think of optional dependencies as "excluded by default."

和scope区分起来, 问题主要在于scope没有提供一种关系, 能够表示我可能用到这个包, 但是概率不大的情况. 比较类似的是`provide`, 但是一个是依赖的范围, 一个是传递的范围
