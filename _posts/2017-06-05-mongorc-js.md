---
layout: post
title:  "配置~/.mongorc.js"
date:  2017-06-05 15:36:02 +0800
categories:
  - Database
tag:
  - database
  - mognodb

---

* content
{:toc}


### MongoDB的mongorc.js的常用配置为
``` js

//查询结果json格式化
DBQuery.prototype._prettyShell = true

//禁用dropDatabase
db.dropDatabase = DB.prototype.dropDatabase = function() {
  print("dropDatabase is disabled on this environment");
};

//查询结果默认行数
DBQuery.shellBatchSize = 20;

//定制查询提示, 可以用来提示数据库前缀
host = db.serverStatus().host;
prompt = function() {
  return db+"@"+host+"$ ";
}

```
