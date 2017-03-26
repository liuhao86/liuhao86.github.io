---
layout: post
title:  "在线调整Redis设置"
date:   2016-06-13 16:04:32 +0800
categories:
  - Ops
tags:
  - notes
  - cache
---

* content
{:toc}

> 很多时候需要调整Redis线上环境设置 **调整后建议立即调整配置文件**, 防止重启后失效

    很多的参数可以调整, `config get *`查看参数名称

### 切换RDB存储到AOF存储
``` shell
redis-cli$ config set save ""

redis-cli$ config set dir "/data/redis/aof/6380/"

redis-cli$ config set appendonly yes
```

### 调整最大内存设置到4G
``` shell
redis-cli$ config set maxmemory 4294967296
```

### 调整内存超过最大内存的淘汰策略
``` shell
redis-cli$ config set maxmemory-policy noeviction
```
- volatile-lru -> remove the key with an expire set using an LRU algorithm
- allkeys-lru -> remove any key according to the LRU algorithm
- volatile-random -> remove a random key with an expire set
- allkeys-random -> remove a random key, any key
- volatile-ttl -> remove the key with the nearest expire time (minor TTL)
- noeviction -> don't expire at all, just return an error on write operations
