---
layout: post
title:  "Linux开发哲学"
date:   2017-04-05 10:12:44 +0800
categories:
  - Linux
tag:
  - linux

---

* content
{:toc}

### Unix特点
* Everything (including hardware) is a file.
  所有的事物（甚至硬件本身）都是一个的文件。

* Configuration data stored in text.
以文本形式储存配置数据。

* Small， single-purpose program.
程序尽量朝向小而单一的目标设计

* Avoid captive user interfaces.
尽量避免令人困惑的用户接口

* Ability to chain program together to perform complex tasks.
将几个程序连结起来，处理大而复杂的工作。


### UNIX的哲学 (Doug McIlroy)
* Write programs that do one thing and do it well。
* Write programs to work together。
* Write programs to handle text streams， because that is a universal interface。

### UNIX的哲学 《The Art of Unix Programming》
* Rule of Modularity： Write simple parts connected by clean interfaces。
* Rule of Clarity： Clarity is better than cleverness。
* Rule of Composition： Design programs to be connected to other programs。
* Rule of Separation： Separate policy from mechanism； separate interfaces from engines。
* Rule of Simplicity： Design for simplicity； add complexity only where you must。
* Rule of Parsimony： Write a big program only when it is clear by demonstration that nothing else will do。
* Rule of Transparency： Design for visibility to make inspection and debugging easier。
* Rule of Robustness： Robustness is the child of transparency and simplicity。
* Rule of Representation： Fold knowledge into data so program logic can be stupid and robust。
* Rule of Least Surprise： In interface design， always do the least surprising thing。
* Rule of Silence： When a program has nothing surprising to say， it should say nothing。
* Rule of Repair： When you must fail， fail noisily and as soon as possible。
* Rule of Economy： Programmer time is expensive； conserve it in preference to machine time。
* Rule of Generation： Avoid hand-hacking； write programs to write programs when you can。
* Rule of Optimization： Prototype before polishing。 Get it working before you optimize it。
* Rule of Diversity： Distrust all claims for “one true way”。
* Rule of Extensibility： Design for the future， because it will be here sooner than you think。


### UNIX的哲学 Mike Gancarz(author of X Windows)
* Small is beautiful。
* Make each program do one thing well。
* Build a prototype as soon as possible。
* Choose portability over efficiency。
* Store data in flat text files。
* Use software leverage to your advantage。
* Use shell scripts to increase leverage and portability。
* Avoid captive user interfaces。
* Make every program a filter。


### The Unix-Haters Handbook
>“The fundamental difference between Unix and the Macintosh operating system is that Unix was designed to please programmers， whereas the Mac was designed to please users。 （Windows， on the other hand， was designed to please accountants。”

本文内容来源[异次元 你不知道的过去 - Linux 系统发展史小览 (与Unix区别科普文)](http://www.iplaysoft.com/p/brief-history-of-linux)
