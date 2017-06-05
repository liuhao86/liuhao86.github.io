---
layout: post
title:  "Intellij IDEA 学习笔记"
date:   2017-04-06 19:21:39 +0800
categories:
  - Linux
tag:
  - linux

---

* content
{:toc}

### 快捷键
* `Option + Command + O` 搜索符号, 包括方法/类
* `Shift+Shift` Search Everywhere, 类, 文件, 系统功能, 插件, 设置
* `Command +E` Recent Files
* `Shift+option + 方向上下` 向上下移动代码行
* `option +方向 ` 快速移动
* `Command +J` template
* `option + command + T` surround with
* `Command +B` usage
* `option command B` impletations
* `Commnad + U` super
* `option + F7` find Usage

### 注意事项
* `command + ;`打开项目设置
* `command + ,`打开偏好设置, 注意
    1. `Version Control`中如果有项目在没有跟踪的状态, 是不会检测到代码更新的
    2. `Build, Execution, Deployment` -> `Maven` -> `ignore Files`这里被忽略的模块是不会在projects里面看到的, 也无法import回来

### 亮点
* Help -> Productivity Guide
* File -> Setting Repository 同步本地配置到远程仓库, 我用的是github, PyCharm也放进去了, 不过不能同一个库,否则会出问题
