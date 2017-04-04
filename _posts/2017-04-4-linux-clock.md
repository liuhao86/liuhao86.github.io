---
layout: post
title:  "Linux时钟"
date:   2017-04-04 14:21:59 +0800
categories:
  - Linux
tag:
  - linux
  - clock

---

* content
{:toc}

本篇文章涉及一下软件或工具:
`clock` `hwclock` `adjtimex` `ntpd` `ntpdate` `sntp` `chronyd` `crontab`

#### ⌘ 一个诡异的问题

线上有10几台服务器, 所有服务器每隔一个小时和ntp服务器同步一次;

>其中193,194等服务器时间正常, 187,207等服务器每次和ntp服务器同步是有46秒差值;

服务器使用crontab做定时调用, 使用ntpdate做时间同步, 同步配置如下:
``` sh
1 */1 * * * /usr/sbin/ntpdate 192.168.8.174
```

>部分服务对时间非常敏感, 要求机器间的时间差不大于500ms, 否则可能引起报警.

某天早上, 服务器开始疯狂报警, 报警说从消息队列消费的事件和放入MQ的时间差差了40s左右, 初步判断是时间不同步的原因, 使用Ansible脚本批量同步所有机器:
``` sh
ansible all -i host_production -m "shell" -a "/usr/sbin/ntpdate 192.168.8.174"
```
得到结果, 187, 207等机器的误差和时间服务器相差较多, 达到46秒. 经过20秒后, 再次执行同步, 发现187,207等机器依然有3秒左右的偏移.

#### ⌘ Linux中时钟和相关工具

#### ⌘ 相关概念
`tick`: 时间单位,以系统定时器的周期性中断为基准, 用户态下内核时钟计数间隔,默认都是100HZ, 单个tick代表了10^4us.

    可以设置每个tick代表的时钟长度，把tick增加1（即增加为10001us）的影响是每天时间快8.64s `24*3600*100*10001/10^6-24*3600=8.64s`。

`ppm`:百万分之一秒，1个PPM增加`24*3600*(10^6+1)/10^6-24*3600=0.0864s`

`freq`:freq增加65536相当于增加1个PPM(1E-6S)。freq增加65536，每天的影响0.0864s/天

|参数|	增加单位数	|1天影响|
|---|---|---|
|freq	|+1	|+0.000001318359375s
|ppm|	+1	|+0.0864s
|tick|	+1	|+8.64s

##### `adjtimex`
使用`adjtimex -c`可以获取时间偏差和推荐的`tick`值.如下:
```
                                                  --- current ---   -- suggested --
cmos time     system-cmos  error_ppm   tick      freq    tick      freq
1491293010      -0.076065
1491293020      -0.085408     -934.3  10000    633200
1491293030      -0.079562      584.6  10000    633200    9994   1642300
1491293040      -0.107722    -2816.0  10000    633200   10028   1682400
```

 当系统时钟较慢, 每次都落后于时间服务器, 可以使用`adjtimex -t 10088`可以将tick调整为某个大于10000的值, 计算方式为`1 tick`表示一天加快8.64s.
>但是当`ntpdate`或者`ntpd`执行时, 可能调整tick的值.

##### `clock`和`hwclock`

使用`clock -c`可以方便的比较硬时钟和系统使用的偏差, 结果如下:
```
hw-time      system-time         freq-offset-ppm   tick
1491292942   1491292941.897342
1491292952   1491292951.918834              2149     21
1491292962   1491292961.939516              2109     21
1491292972   1491292971.960110              2092     21
```

#### ⌘ 可能遇到的问题
1. 每次使用adjtimex设置tick为10000后, 会被调整为9167
`ntpdate`旧版有问题,并参考下面的**问题2**中的chronyd.

    推荐升级ntpdate:
    ``` sh
    yum install ntp && yum update ntp
    ```

    或者使用`sntp -sS`或者使用`ntpd -gqd`,参见[Deprecating ntpdate](http://support.ntp.org/bin/view/Dev/DeprecatingNtpdate),

2. ntpd服务看起来没有启动
    使用`systemctl status ntpd`查看服务状态时,发现服务并没有启动,`crontab`中也没有配置,ntpd是如何提供服务的呢.

    在`Redhat`Family的系统中(Fedora, Centos), 有另外一个应用`chronyd`,配置文件位于`/etc/chrony.conf`, 会作为守护进程调整内核中运行的系统时钟和时钟服务器同步
