---
layout: post
title:  "Scala学习笔记"
date:  2017-06-22 20:27:51 +0800
categories:
  - Scala
tag:
  - scala

---

* content
{:toc}


### Scala学习笔记
Call by value: default
Call by Name: =>    def constOne(x:Double,y: => Int)=1


def plus(x:Int):Int= x+plus(x-1)

`def` by name, `val` by value

``` scala
def and(x:Boolean,y:=>Boolean) = if(x) y else false

def or(x:Boolean, y: =>Boolean) = if(x)true else y
```
```
def sqrt(guess: double, x: Double): Double = if(guess,x) (guess+x)/2  else (guess+sqrt(quess))/x
```
gcd great common divitor 最大公因子

`@tailrec`if implementation is not tail recursive, error will be issued.

tail recursion 尾递归 could avoid statckoverflow
factorial
