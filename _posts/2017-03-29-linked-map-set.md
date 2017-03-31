---
layout: post
title:  "Java Maps"
date:   2017-03-28 10:24:47 +0800
categories:
  - Java
tag:
  - java

---

* content
{:toc}

#### LinkedHashMap
`LinkedHashMap`和`HashMap`相比有几个特点:
* 有序性. 可以在构造时指定是按照`插入`顺序排序, 或者是按照`访问`顺序排序.
* 由于维护了顺序, `LinkedHashMap`要略慢于`HashMap`, 但是迭代访问速度稍快

#### initialCapacity 和 load factor
Map中有几个数字需要注意, 分别是:
* initCapacity
* loadFactor
* threshold
* size
* capacity

默认`initialCapacity`=16, `loadFactor`=0.75
capacity = min(N),其中 N=2^x, N>=initialCapacity

假设new HashMap(9); 则initialCapacity=9, loadFactor=0.75, capacity=2^N=16(N=4),  threshold=capacity*0.75=12

当放入(16*0.75=12)个对象时,threshold=12;
继续放入一个时, threshold=2^N**0.75=24(N=5)

所以如果需要一个固定大小的Map, 不需要进行resize时,应该这样声明:
``` java
//loadFactory=1, 等于容量的时候再扩张
new HashMap(M, 1f);
```
或者
``` java
//申请较大的容量, 使之达不到0.75
int initCapacity = (int)(M*1f/0.75+1f);
new HashMap(initCapacity);
```

假设现在需要一个768长度的Map, 以后很少可能继续添加元素

如果声明为`new HashMap(768, 1f);`,则capacity为1024, 有效比为75%;

如果声明为`new HashMap((int)(768*1f/0.75+1f))`, 1025,capacity为2048, 有效率为37.5%;

所以对基本不变化的Map来说, loadFactor为1
