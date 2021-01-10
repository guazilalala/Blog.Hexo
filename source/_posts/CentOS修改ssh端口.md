---
title: CentOS修改ssh端口
date: 2021-01-10 12:38:56
tags: Linux
categories: Linux
---

### 备份SSH配置文件

``` shell
[root@wing-centos ~]# cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```

### 修改配置文件

``` shell
[root@wing-centos ~]# vim /etc/ssh/sshd_config
```

将端口22改为新端口

``` shell
# If you want to change the port on a SELinux system, you have to tell
# SELinux about this change.
# semanage port -a -t ssh_port_t -p tcp #PORTNUMBER
#
Port 28818
#AddressFamily any
#ListenAddress 0.0.0.0
#ListenAddress ::
```

### 允许新端口通过防火墙

``` shell
[root@wing-centos ~]# firewall-cmd --permanent --zone=public --add-port=28818/tcp
```

``` shell
[root@wing-centos ~]# firewall-cmd --reload
```

### 重启sshd服务并验证

重启sshd服务

``` shell
[root@wing-centos ~]# systemctl restart sshd.service
```

验证SSH现在是否在新端口上运行

``` shell
[root@wing-centos ~]# ss -tnlp | grep ssh
```

连接时通过-p参数指定端口

``` shell
[root@wing-centos ~]# ssh root@192.168.10.11 -p 28818
```
