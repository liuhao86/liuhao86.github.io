---
layout: post
title:  "Kafka Consumer实践"
date:   2017-03-26 18:27:47 +0800
categories:
  - 架构
tag:
  - kafka
---


#### 背景
* 生产环境采用默认5个分区的设置
* kafka已经经过一层封装, 实现了自动增减consumer的逻辑

#### 问题
* JVM进程kill -15 无法自动退出, 需要-9杀掉, 导致连接层的Session数据不一致
* consumer中有一堆的无效consumer, 专门做探测用
* __offset_commit topic增长飞快

#### 解决方案
通过网上查了好多别人的资料, 在kafka的topic的partition的设置和consumer的管理上, 没有提到过, 可能大家已经使用了我找到的方案.

##### 去除自动探测consumer分配的逻辑
首先说下探测的逻辑:
* 监听partition分配的事件,
* 在分配事件触发后, 获取分配的分区数
* 如果比当前机器里面的consumer多, 增加consumer, 反之减少

在分布式系统中, 也可以工作的不错, 最终保证consumer数量和partition保持一致.问题有两个:
1. 从数量不一致到数量一致, 需要增减多次, 可能会导致commit的offset出现问题, 导致重复消费或者漏掉消息;
2. 需要单独的consumer做探测, 和普通的消费事件的分离开,在查看consumer列表时, 会看到一批探测的consumer

##### 增加partition数量
不能自动管理consumer数量, 就换一个方案, 管理partition.
* 增加partition数量, 使之远大于消费的jvm进程,
* 每个进程内有固定个数的consumer

如partition数量为36, 每个进程内固定3个consumer,随着机器的增加, 分配的情况如下:

|JVM进程数|总的consumer数量|每个consumer数量|
|---------------|-------------------------|--------------------------|
|1| 3| 12|
|2|6|6|
|3|9|4|
|4|12|3|
|5|15|2-3|
|6|18|2|
|7|21|1-2|

这种配置下, 大于12个进程的时候,就会存在有个机器上的consumer闲置的情况. 可以调整每个进程consumer数量和partition的数量.

#### 其他
1. consumer.poll(10000), 这个参数是拉取记录时, 没有记录的等待时间, 我觉得不必设置太短, 比如100(ms), 可以适当增加
2. kafka-manager是我用过最好的管理监控工具
