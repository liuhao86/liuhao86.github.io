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
