---
layout: centos
title: 在CentOS8安装RabbitMQ
date: 2021-01-02 20:44:05
tags: Linux
categories: Linux
---

### 安装Erlang

由于RabbitMQ是基于Erlang语言开发，所以在安装RabbitMQ之前，需要先安装Erlang。

``` shell
[root@wing-centos Downloads]# wget https://github.com/rabbitmq/erlang-rpm/releases/download/v23.2.1/erlang-23.2.1-1.el8.x86_64.rpm
[root@wing-centos Downloads]# dnf install -y erlang-21.3.8.6-1.el7.x86_64.rpm
```

### 添加RabbitMq

到<https://packagecloud.io/rabbitmq/rabbitmq-server/packages/el/8/rabbitmq-server-3.8.9-1.el8.noarch.rpm>下载最新rpm包

``` shell
[root@wing-centos Downloads]# wget --content-disposition https://packagecloud.io/rabbitmq/rabbitmq-server/packages/el/8/rabbitmq-server-3.8.9-1.el8.noarch.rpm/download.rpm
```

安装

``` shell
[root@wing-centos Downloads]# dnf install rabbitmq-server-3.8.9-1.el8.noarch.rpm
```

查看包的详细信息

``` shell
[root@wing-centos Downloads]# rpm -qi rabbitmq-server
Name        : rabbitmq-server
Version     : 3.8.9
Release     : 1.el8
Architecture: noarch
Install Date: Sat 02 Jan 2021 09:51:24 PM CST
Group       : Development/Libraries
Size        : 16062796
License     : MPLv2.0 and MIT and ASL 2.0 and BSD
Signature   : RSA/SHA256, Fri 25 Sep 2020 02:52:51 AM CST, Key ID 6b73a36e6026dfca
Source RPM  : rabbitmq-server-3.8.9-1.el8.src.rpm
Build Date  : Fri 25 Sep 2020 02:52:50 AM CST
Build Host  : 7d818ab8-a4b5-4a3a-406c-c1fc4634db09
Relocations : (not relocatable)
URL         : https://www.rabbitmq.com/
Summary     : The RabbitMQ server
Description :
RabbitMQ is an open source multi-protocol messaging broker.
```

### 配置防火墙并启动RabbitMQ

防火墙添加5672和15672端口，15672是后台管理地址的端口

``` shell
[root@wing-centos Downloads]# firewall-cmd --add-port={5672,15672}/tcp --permanent
success
[root@wing-centos Downloads]# firewall-cmd --reload
success
```

启动RabbitMQ并设置自动启动

``` shell
[root@wing-centos Downloads]# systemctl start rabbitmq-server.service
[root@wing-centos Downloads]# systemctl enable rabbitmq-server.service
Created symlink /etc/systemd/system/multi-user.target.wants/rabbitmq-server.service → /usr/lib/systemd/system/rabbitmq-server.service.
```

### 安装后台管理

安装RabbitMQ RabbitMQ Management

``` shell
[root@wing-centos ~]# rabbitmq-plugins enable rabbitmq_management
```

设置管理员用户，后台默认有一个guest用户，但只能够本地访问，远程访问需要再添加用户。

``` shell
rabbitmqctl add_user username passwd  //添加用户，后面两个参数分别是用户名和密码
rabbitmqctl set_permissions -p / username ".*" ".*" ".*"  //添加权限
rabbitmqctl set_user_tags username administrator  //修改用户角色,将用户设为管理员
```

> 参考：
> https://www.osradar.com/how-to-install-rabbitmq-on-rhel-8-centos-8
> https://www.cnblogs.com/cwp-bg/p/10070467.html
