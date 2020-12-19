---
title: CentOS7升级CentOS8
date: 2020-11-28 14:42:28
tags: Linux
categories: Linux
---

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

使用dfn安装CentOS8发行包
{% codeblock %}
[root@wing-centos ~]# dnf install -y https://mirrors.aliyun.com/centos/8/BaseOS/x86_64/os/Packages/centos-repos-8.2-2.2004.0.1.el8.x86_64.rpm https://mirrors.aliyun.com/centos/8/BaseOS/x86_64/os/Packages/centos-release-8.2-2.2004.0.1.el8.x86_64.rpm https://mirrors.aliyun.com/centos/8/BaseOS/x86_64/os/Packages/centos-gpg-keys-8.2-2.2004.0.1.el8.noarch.rpm
{% endcodeblock %}

更换阿里云下载源，国外下载源太慢了，换成阿里的， 更换前先备份一下repo文件
{% codeblock %}
[root@wing-centos ~]# cd /etc/yum.repos.d/
[root@wing-centos yum.repos.d]# mv /etc/yum.repos.d/CentOS-AppStream.repo /etc/yum.repos.d/CentOS-AppStream.repo.bak
[root@wing-centos yum.repos.d]# mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
[root@wing-centos yum.repos.d]# mv /etc/yum.repos.d/CentOS-centosplus.repo /etc/yum.repos.d/CentOS-centosplus.repo.bak
[root@wing-centos yum.repos.d]# mv /etc/yum.repos.d/CentOS-Extras.repo /etc/yum.repos.d/CentOS-Extras.repo.bak
[root@wing-centos yum.repos.d]# mv /etc/yum.repos.d/CentOS-PowerTools.repo /etc/yum.repos.d/CentOS-PowerTools.repo.bak
{% endcodeblock %}

下载阿里云存储库文件
{% codeblock %}
[root@wing-centos yum.repos.d]# wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-8.repo
--2020-11-30 12:36:38--  https://mirrors.aliyun.com/repo/Centos-8.repo
Resolving mirrors.aliyun.com (mirrors.aliyun.com)... 171.220.238.248, 118.123.2.184, 110.188.26.244, ...
Connecting to mirrors.aliyun.com (mirrors.aliyun.com)|171.220.238.248|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2595 (2.5K) [application/octet-stream]
Saving to: ‘/etc/yum.repos.d/CentOS-Base.repo’

100%[==================================================================================================================>] 2, 595       --.-K/s   in 0s

2020-11-30 12:36:39 (436 MB/s) - ‘/etc/yum.repos.d/CentOS-Base.repo’ saved [2595/2595]
{% endcodeblock %}

开始升级
{% codeblock %}
[root@wing-centos ~]# dnf --allowerasing distro-sync -y
{% endcodeblock %}

### 查看系统版本

重启主机后查看系统版本是否已经更新了
{% codeblock %}
[root@wing-centos ~]# cat /etc/redhat-release
{% endcodeblock %}

### 清除dnf缓存

{% codeblock %}
[root@wing-centos ~]# dnf clean all
Repository centosplus is listed more than once in the configuration
Repository extras is listed more than once in the configuration
36 files removed
{% endcodeblock %}

> 参考：
> https://www.tecmint.com/upgrade-centos-7-to-centos-8/
> https://developer.aliyun.com/mirror/centos?spm=a2c6h.13651102.0.0.3e221b11dZPNeJ
> https://blog.csdn.net/u010039418/article/details/108588941
