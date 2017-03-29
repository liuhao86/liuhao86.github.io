---
layout: post
title:  "SDKMan实践笔记"
date:   2017-03-28 10:24:47 +0800
categories:
  - Ops
tag:
  - ops

---

* content
{:toc}

#### 安装SDKMAN!

``` sh
curl -s "https://get.sdkman.io" | bash
```
> 我遇到了安装失败, 可能由于部分资源访问不到, 翻过GFW之后安装成功了, 如果想看到详细步骤, 就先`wget`, 再`sh`

#### 查找
``` sh
sdk list| sdk ls
sdk ls java
```


#### 安装环境

``` sh
sdk install java | sdk i java
sdk i java 8u121 [path]
```

#### 切换版本
``` sh
# 查看版本
sdk c
sdk current java
sdk c java
```

``` sh
# 当前命令行生效
sdk use java 8u121
sdk u java 8u121
```
``` sh
# 永久切换
sdk default java 8u121
sdk d java 8u121
```

#### 卸载环境
``` sh
sdk uninstall java
sdk rm java
sdk rm java 8u121
```

#### 配置
配置文件位于`~/.sdkman/etc/config`

#### 问题

##### 安装软件失败
`curl: option --progress-bar --location --cookie oraclelicense=accept-securebackup-cookie: is unknown`

>可能原因是在zsh等shell中运行, 切换到bash中实行`sdk i [candicate]`
