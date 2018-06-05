---
layout: post
title:  "SpringData-MongoDB查询速度慢的一种原因"
date:  2018-05-30 17:06:45 +0800
categories:
  - SpringData
  - MongoDB
tag:
  - springboot
  - mongodb

---

* content
{:toc}


### 问题说明
1. spring-data-mongodb一个很简单的查询, 耗费时间1.6秒左右,直接在数据库查询, 仅需要5ms.
2. 连续查询仅第一次慢, 后续都比较快约145ms.

### 问题解决
Spring-data-mongodb, 在每一个文档上增加了属性`_class`, 由于项目重构,包名或者类型变化,导致`_class`的值不匹配

这种情况下, 会导致每次调用ClassUtils.forName() and ClassLoader.load()加载类.

这种情况Spring-data应该可以避免,但是目测他们并没有做事情.虽然已经在调用的时候传入期望的类名, 估计在子类处理方面存在问题
