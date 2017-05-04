---
layout: post
title:  "Maven Release Plguin实战"
date:  2017-04-26 18:42:13 +0800
categories:
  - Linux
tag:
  - linux

---

* content
{:toc}


### net

1. web 容器只监听 ipv6端口, 不监听 ipv4
```
net.ipv6.conf.all.disable_ipv6=1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.eth0.disable_ipv6 = 1 #eth0为网卡
```
