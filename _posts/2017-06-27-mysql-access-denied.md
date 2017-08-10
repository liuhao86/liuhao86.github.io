---
layout: post
title:  "MySQL Access Denied for user '...'@'localhost' (using password: YES)"
date:  2017-06-27 13:30:28 +0800
categories:
  - database
  - mysql
tag:
  - database

---

* content
{:toc}


### Access Denied for localhost connection
``` shell
[root@gvma logs]# mysql -u opcounter -p
Enter password:
ERROR 1045 (28000): Access denied for user 'opcounter'@'localhost' (using password: YES)
```
本人遇到的问题和这个[MySQL ERROR 1045 (28000): Access denied for user 'bill'@'localhost' (using password: YES)](https://stackoverflow.com/questions/10299148/mysql-error-1045-28000-access-denied-for-user-billlocalhost-using-passw)描述的问题相同
因为存在匿名用户`''@'localhost'` `''@'127.0.0.1'`
删除这两个用户, 然后`FLUSH PRIVILEGES`后问题解决
