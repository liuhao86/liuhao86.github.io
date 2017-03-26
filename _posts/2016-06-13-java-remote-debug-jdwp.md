---
layout: post
title:  "Java远程调试JDWP"
date:   2015-06-13 16:04:32 +0800
categories:
  - Java
tags:
  - notes
  - java
---

* content
{:toc}

> 使用JDWP可以方便快捷的对非Eclipse环境下启动的应用进行调试

```
-Xdebug -Xrunjdwp:transport=dt_socket,address=8787,server=y,suspend=n
```
- address: 指定端口即可,监听所有Interface
- suspend: [y|n] 表示是否等待jdwp调试端连接上之后, 再继续启动, 可用于调试启动过程
