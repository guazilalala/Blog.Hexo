---
title: 设置ssh免密登录linux
date: 2020-12-08 11:52:14
tags: Linux
categories: Linux
---


### 使用ssh-keygen创建rsa加密的私钥和公钥

``` Shell
wing@MTT-Tangy:~/.ssh$ ssh-keygen -t rsa
```

### 将公钥追加到远端authorized_keys文件

``` Shell
wing@MTT-Tangy:~/.ssh$ cat id_rsa.pub >>root@ root@192.168.10.12:.ssh/authorized_keys
```
