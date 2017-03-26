---
layout: post
title:  "使用linux命令分析Nginx日志"
date:   2017-02-18 13:31:01 +0800
categories:
  - Ops
tags:
  - notes
  - nginx
---

* content
{:toc}


#### 过滤出所有Http Status Code大于399的请求
> 按照配置的accesslog日志的样式对`$n`进行调整

``` sh
tail -1000 access.log |awk '{if($9>399)printf "%s %s %s\n", $9,$4,$7}'
```

#### 过滤响应时间大于n秒的请求
``` sh
tail -100000 access.log |grep version/upesn/esn |awk '{if($10>0.8) printf "%s %s %s\n" ,$4, $7, $10}'
```

#### 获取请求的响应时间分布

``` sh
tail -100000 access.log |grep version/upesn/esn |awk '{print $10}'|sort|uniq -c
```
