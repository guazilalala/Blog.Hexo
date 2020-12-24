---
title: 保持SSH连接
date: 2020-12-24 23:33:59
tags: Linux
categories: Linux
---


### 配置服务端

修改服务端配置文件

``` shell
[root@wing-centos ~]# vim /etc/ssh/sshd_config
```

添加以下选项

``` shell
ClientAliveInterval 30
ClientAliveCountMax 60
```

### 配置客户端

修改客户端配置文件

``` shell
wing@Wing-Home:/mnt/c/Users/Feet$ vim /etc/ssh/ssh_config
```

添加以下选项

``` shell
ServerAliveInterval 30
ServerAliveCountMax 60
```

表示本地 ssh 每隔 30s 向 server 端 sshd 发送 keep-alive 包，如果发送 60 次，server 无回应断开连接。

或者可以通过连接时添加选项

``` shell
ssh root@192.168.10.11 -o ServerAliveInterval=240
```
