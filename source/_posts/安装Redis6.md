---
title: 安装Redis6
date: 2020-12-31 15:05:02
tags: Linux
categories: Linux
---


### 下载压缩包并解压

``` shell
[root@wing-centos Downloads]# https://download.redis.io/releases/redis-6.0.9.tar.gz

[root@wing-centos Downloads]# tar -zxvf redis-6.0.9.tar.gz
``` 

### 编译安装

``` shell
[root@wing-centos Downloads]# make && make install PREFIX=/usr/local/redis
```

### 配置后台启动

将配置文件拷贝到安装目录
``` shell
[root@wing-centos Downloads]# cp /data/Downloads/redis-6.0.9/redis.conf /usr/local/redis/bin/
```

修改 redis.conf 文件，把 daemonize no 改为 daemonize yes

### 配置远程连接

修改 redis.conf 文件，把 protected-mode yes 改为 protected-mode no
修改 redis.conf 文件，把 bind 127.0.0.1 改为 bind 0.0.0.0 或者注释掉

防火墙开始端口
``` shell
[root@wing-centos Downloads]# firewall-cmd --zone=public --add-port=6379/tcp --permanent
[root@wing-centos Downloads]# firewall-cmd --reload
```

### redis-cli添加到环境变量

修改文件 
``` shell
[root@wing-centos Downloads]# vim ~/.bash_profile
```
添加配置
``` shell
export REDIS_HOME=/usr/local/redis
export PATH=$PATH:$REDIS_HOME/bin
```
使用配置生效
``` shell
[root@wing-centos Downloads]# source ~/.bash_profile
```

### 通过 systemctl 配置自动启动

修改 redis.conf 文件，把 supervised no 改为 supervised systemd

在 /etc/systemd/system 目录，创建 redis-server.service 文件，并添加以下内容：
```
[Unit]
Description=Redis data structure server
Documentation=https://redis.io/documentation

[Service]
ExecStart=/usr/local/redis/bin/redis-server /usr/local/redis/bin/redis.conf
ExecStop=/usr/local/redis/bin/redis-cli shutdown
Restart=always
LimitNOFILE=10032
NoNewPrivileges=yes
Type=forking
UMask=0077

[Install]
WantedBy=multi-user.target
``` 

重新加载service文件并启动服务
``` shell
[root@wing-centos system]#  systemctl daemon-reload
[root@wing-centos bin]# systemctl start redis-server.service
```

设置开机启动
``` shell
[root@wing-centos bin]# systemctl enable redis-server.service
```

### 确认 redis 进程是否启动
``` shell
[root@wing-centos Downloads]# ps -ef | grep redis
```


> 参考：
> https://my.oschina.net/kaisesai/blog/4277630
