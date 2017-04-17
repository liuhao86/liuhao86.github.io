---
layout: post
title:  "Git常用命令和技巧"
date:   2017-03-27 15:45:47 +0800
categories:
  - Git
tag:
  - git
---

* content
{:toc}

#### 将已经提交的文件, 从Git中删除
``` sh
git rm [-r] --cached 文件[夹]路径
```

#### 配置autocrlf统一换行符
``` sh
# windows
git config --global core.autocrlf true

# Mac/Linux
git config --global core.autocrlf input
```
这样在windows迁出时自动替换为CRLF, 提交时自动替换为LF

在Mac/Linux等Unix系统上, 迁出时保持原样, 提交时将CRLF自动替换为LF

#### 统一的.gitignore文件

通过变量`core.excludesFile`控制机器下git配置, 等同于`~/.gitconfig`下配置

``` config
[core]
	excludesFile = ~/.config/git/ignore
```

git官方的说明:
>Specifies the pathname to the file that contains patterns to describe paths that are not meant to be tracked, in addition to .gitignore (per-directory) and .git/info/exclude. Defaults to $XDG_CONFIG_HOME/git/ignore. If $XDG_CONFIG_HOME is either not set or empty, $HOME/.config/git/ignore is used instead. See gitignore(5).


##### 将当前目录下的`.gitignore`批量复制到当前目录下多个子文件夹
``` sh
for dir in $('ls');do cp -vf .gitignore $dir;done
```
