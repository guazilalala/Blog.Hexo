---
title: CentOS7升级CentOS8
date: 2020-11-28 14:42:28
tags: Linux
categories: Linux
---

## 环境

### 原版本

``` shell
[root@wing-centos ~]# cat /etc/redhat-release && uname -r
CentOS Linux release 7.6.1810 (Core)
3.10.0-1062.12.1.el7.x86_64
```

### 新版本

``` shell
[root@wing-centos ~]# cat /etc/redhat-release && uname -r
CentOS Linux release 8.3.2011
5.10.1-1.el8.elrepo.x86_64
```

## 升级步骤

### 安装EPEL存储库

``` shell
[root@wing-centos ~]# yum install epel-release -y
```

### 安装yum-utils

``` shell
[root@wing-centos ~]# yum install yum-utils -y
```

``` shell
[root@wing-centos ~]# yum install rpmconf -y
```

``` shell
[root@wing-centos ~]# rpmconf -a
Configuration file '/etc/cloud/cloud.cfg'
-rw-r--r-- 1 root root 1234 Aug  8  2019 /etc/cloud/cloud.cfg.rpmnew
-rw-r--r-- 1 root root 2646 Jul  9 23:41 /etc/cloud/cloud.cfg

 ==> Package distributor has shipped an updated version.
   What would you like to do about it ?  Your options are:
    Y or I  : install the package maintainer's version
    N or O  : keep your currently-installed version
      D     : show the differences between the versions
      M     : merge configuration files
      Z     : background this process to examine the situation
      S     : skip this file
 The default action is to keep your current version.
*** aliases (Y/I/N/O/D/M/Z/S) [default=N] ?
Your choice: N
Configuration file '/etc/cloud/cloud.cfg'
-rw-r--r-- 1 root root 1416 Jul 15  2019 /etc/cloud/cloud.cfg.rpmsave
-rw-r--r-- 1 root root 2646 Jul  9 23:41 /etc/cloud/cloud.cfg

 ==> Package distributor has shipped an updated version.
 ==> Maintainer forced upgrade. Your old version has been backed up.
   What would you like to do about it?  Your options are:
    Y or I  : install (keep) the package maintainer's version
    N or O  : return back to your original file
      D     : show the differences between the versions
      M     : merge configuration files
      Z     : background this process to examine the situation
      S     : skip this file
 The default action is to keep package maintainer's version.
*** aliases (Y/I/N/O/M/D/Z/S) [default=Y] ?
Your choice: Y
```

``` shell
[root@wing-centos ~]# package-cleanup --leaves
[root@wing-centos ~]# package-cleanup --orphans
```

### 安装dnf

CentOS 8 默认的包管理工具已经换成了dnf，使用方法基本上和yum一样的。
{% codeblock %}
[root@wing-centos ~]# yum install dnf -y
{% endcodeblock %}

### 删除yum

虽然CentOS8还可以使用yum，但是有dnf已经足够了，还是删了吧。
{% codeblock %}
[root@wing-centos ~]# dnf -y remove yum yum-metadata-parser
[root@wing-centos ~]# rm -rf /etc/yum
{% endcodeblock %}

### 升级CentOS 7 到 CentOS 8

升级包
{% codeblock %}
[root@wing-centos ~]# dnf upgrade -y
{% endcodeblock %}

安装CentOS8发行包
{% codeblock %}
[root@wing-centos ~]# dnf install -y http://mirror.centos.org/centos/8/BaseOS/x86_64/os/Packages/centos-linux-repos-8-2.el8.noarch.rpm http://mirror.centos.org/centos/8/BaseOS/x86_64/os/Packages/centos-linux-release-8.3-1.2011.el8.noarch.rpm http://mirror.centos.org/centos/8/BaseOS/x86_64/os/Packages/centos-gpg-keys-8-2.el8.noarch.rpm
{% endcodeblock %}

升级EPEL储存库

``` shell
[root@wing-centos ~]# rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
[root@wing-centos ~]# dnf upgrade -y epel-release
```

清除dnf缓存

``` shell
[root@wing-centos ~] dnf clean all
```

删除冲突组件，有些组件会有冲突，要先卸载。

``` shell
[root@wing-centos ~] rpm -e kernel sysvinit-tools gdbm kexec-tools systemd-sysv python3 --nodeps
```

如果有多个内核版本需要单独卸载

``` shell
[root@wing-centos ~]# rpm -e kernel-3.10.0-1062.12.1.el7.x86_64
[root@wing-centos ~]# rpm -e kernel-3.10.0-1127.19.1.el7.x86_64
```

为了下载速度快一点，使用阿里云镜像

``` shell
[root@wing-centos yum.repos.d]# curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-8.repo
[root@wing-centos yum.repos.d]# sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo
[root@wing-centos yum.repos.d]# dnf install -y https://mirrors.aliyun.com/epel/epel-release-latest-8.noarch.rpm
[root@wing-centos yum.repos.d]# sed -i 's|^#baseurl=https://download.fedoraproject.org/pub|baseurl=https://mirrors.aliyun.com|' /etc/yum.repos.d/epel*
[root@wing-centos yum.repos.d]# sed -i 's|^metalink|#metalink|' /etc/yum.repos.d/epel*
```

开始升级
{% codeblock %}
[root@wing-centos ~]# dnf --allowerasing distro-sync -y
{% endcodeblock %}

### 升级到最新内核

``` shell
[root@wing-centos ~] dnf install -y https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm
[root@wing-centos ~] dnf --enablerepo=elrepo-kernel -y install kernel-ml
```

### 查看系统版本

重启主机后查看系统版本是否已经更新了
{% codeblock %}
[root@wing-centos ~]# cat /etc/redhat-release &&  uname -r
{% endcodeblock %}


> 参考：
> https://www.tecmint.com/upgrade-centos-7-to-centos-8/
> https://developer.aliyun.com/mirror/centos?spm=a2c6h.13651102.0.0.3e221b11dZPNeJ
> https://blog.csdn.net/u010039418/article/details/108588941
> https://blog.yowko.com/centos7-upgrade-centos8-linux-kernel5/
