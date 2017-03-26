---
layout: post
title:  "TCPCopy实战"
date:   2017-02-18 10:31:01 +0800
categories:
  - Ops
tags:
  - notes
  - tcpcopy
  - test
---

* content
{:toc}

### Background

* Test machine:  **172.20.19.197, 172.20.19.198**
* Test machine nginx server: ** 172.20.19.196 **
* Assistant test machine: **172.20.19.172**
* Production machine: **172.20.19.187**
* Sub-network not exists: 172.20.190.0/24


### notice
* can not capture traffics in lvs/keepalived server, real server is required

## Actions

### 1. compile tcpcopy

* Assistant test machine:  **172.20.19.172**
``` sh
git clone git://github.com/session-replay-tools/intercept.git
```
``` sh
yum install -y libpcap libpcap-devel
```
``` sh
./configure && make && make install
```

* Production machine: **172.20.19.187**
``` sh
git clone git://github.com/session-replay-tools/tcpcopy.git
```
``` sh
yum install -y libpcap libpcap-devel
```
``` sh
./configure && make && make install
```

### 2. transfer database
* Export data from production environment
* Import into test database
* delete security-sensitive data, for example:
  * **iOS device token**, it's very important

### 3. configure and start tcpcopy

* Test machine:  **172.20.19.197**
```sh
 route add -net 172.20.190.0 netmask 255.255.255.0 gw 172.20.19.172
```

#### For REST API

* Assistant test machine:  **172.20.19.172**
```sh
intercept -i eth0 -F 'tcp and src port 8092' -d -p 36524
```

* Production machine: **172.20.19.187**
```sh
./tcpcopy -x 9090-172.20.19.196:80 -s 172.20.19.172 -c 172.20.190.x -p 36524
```

#### For TCP Socket
* Assistant test machine:  **172.20.19.172**
```sh
intercept -i eth0 -F 'tcp and src port 5227' -d -p 36525
```

* Production machine: **172.20.19.187**
```sh
./tcpcopy -x 5227-172.20.19.196:5227 -s 172.20.19.172 -c 172.20.190.x -p 36525
```

#### For TCP Security Socket
* Assistant test machine:  **172.20.19.172**
```sh
intercept -i eth0 -F 'tcp and src port 5223' -d -p 36526
```

* Production machine: **172.20.19.187**
```sh
./tcpcopy -x 5223-172.20.19.196:5223 -s 172.20.19.172 -c 172.20.190.x -p 36526
```

#### For HTTP-BIND
* Assistant test machine:  **172.20.19.172**
```sh
intercept -i eth0 -F 'tcp and src port 7070' -d -p 36527
```

* Production machine: **172.20.19.187**
```sh
./tcpcopy -x 7070-172.20.19.196:7070 -s 172.20.19.172 -c 172.20.190.x -p 36527
```
