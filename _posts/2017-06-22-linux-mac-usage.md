---
layout: post
title:  "Linux & Mac 使用技巧备忘"
date:  2017-06-22 20:27:51 +0800
categories:
  - Linux
tag:
  - linux

---

* content
{:toc}


### Shell
shell有两种：`登录式shell`与`非登录式shell`
* 登录(login)式shell就是在你打开shell要求你输入用户名与密码的shell，我们在使用桌面Linux时一般只在登录时接触到这种shell
* 我们进入系统后，再打开的Terminal就是非登录式shell了

这两种 shell 读取配置的文件是不同的:
* 登录式Shell启动时会去读取~/.profile文件（Redhat、Mac上为 ~/.bash_profile）
* 非登录式Shell会去读取~/.bashrc文件

> Ansible执行远程命令的时候, 有时间会遇到明明`java -version`是安装的最新的Java, 但是`Ansible`启动的应用却是用的alternatives下的Java

### 快捷键
* 强制关闭程序 Command + Option + esc

### `brew cask`
`brew tap caskroom/cask`
