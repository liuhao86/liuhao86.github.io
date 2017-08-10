---
layout: post
title:  "IE跨域Cookie读取问题"
date:  2017-06-27 13:30:28 +0800
categories:
  - cookie
tag:
  - ie

---

* content
{:toc}


### 问题说明
页面A通过iFrame嵌入了另外一个页面B,页面B通过SSO登录到系统, 中间经过多次redirect
在Chrome等浏览器表现正常, IE则跳转到登录页面

### 问题分析
第一感觉就是Cookie读取或者写入除了问题
再感觉就是跨域的问题

* 解决IE跨域情况下, 读取Cookie失败的问题, 增加response header `P3P`, fiddler Script如下
oSession.oResponse["P3P"]="CP='IDC DSP COR ADM DEVi TAIi PSA PSD IVAi IVDi CONi HIS OUR IND CNT'";

再有就是降低IE的安全级别
