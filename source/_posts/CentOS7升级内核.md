---
title: CentOS7升级到最新核
date: 2020-11-27 22:55:50
tags: Linux
categories: Linux
---

### 查看当前内核版本

{% codeblock lang:shell %}
[root@wing-centos ~]# uname -a
Linux wing-centos 3.10.0-1062.12.1.el7.x86_64 #1 SMP Tue Feb 4 23:02:59 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
{% endcodeblock %}

### 更新yum源仓库

{% codeblock lang:shell %}
$ yum -y update
{% endcodeblock %}

### 安装EPEL存储库

导入EPEL存储库的密钥
{% codeblock lang:shell %}
[root@wing-centos ~]# rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
{% endcodeblock %}

安装EPEL存储库的yum源
{% codeblock lang:shell %}
[root@wing-centos ~]# rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
Retrieving http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
Preparing...                          ################################# [100%]
Updating / installing...
   1:elrepo-release-7.0-3.el7.elrepo  ################################# [100%]
{% endcodeblock %}

### 查看可用的系统内核包

{% codeblock lang:shell %}
[root@wing-centos rpm-gpg]# yum --disablerepo="*" --enablerepo="elrepo-kernel" list available
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * elrepo-kernel: hkg.mirror.rackspace.com
elrepo-kernel                                                                                    | 2.9 kB  00:00:00
elrepo-kernel/primary_db                                                                         | 1.9 MB  00:00:00
Available Packages
elrepo-release.noarch                                       7.0-5.el7.elrepo                               elrepo-kernel
kernel-lt.x86_64                                            4.4.246-1.el7.elrepo                           elrepo-kernel
kernel-lt-devel.x86_64                                      4.4.246-1.el7.elrepo                           elrepo-kernel
kernel-lt-doc.noarch                                        4.4.246-1.el7.elrepo                           elrepo-kernel
kernel-lt-headers.x86_64                                    4.4.246-1.el7.elrepo                           elrepo-kernel
kernel-lt-tools.x86_64                                      4.4.246-1.el7.elrepo                           elrepo-kernel
kernel-lt-tools-libs.x86_64                                 4.4.246-1.el7.elrepo                           elrepo-kernel
kernel-lt-tools-libs-devel.x86_64                           4.4.246-1.el7.elrepo                           elrepo-kernel
kernel-ml.x86_64                                            5.9.11-1.el7.elrepo                            elrepo-kernel
kernel-ml-devel.x86_64                                      5.9.11-1.el7.elrepo                            elrepo-kernel
kernel-ml-doc.noarch                                        5.9.11-1.el7.elrepo                            elrepo-kernel
kernel-ml-headers.x86_64                                    5.9.11-1.el7.elrepo                            elrepo-kernel
kernel-ml-tools.x86_64                                      5.9.11-1.el7.elrepo                            elrepo-kernel
kernel-ml-tools-libs.x86_64                                 5.9.11-1.el7.elrepo                            elrepo-kernel
kernel-ml-tools-libs-devel.x86_64                           5.9.11-1.el7.elrepo                            elrepo-kernel
perf.x86_64                                                 5.9.11-1.el7.elrepo                            elrepo-kernel
python-perf.x86_64                                          5.9.11-1.el7.elrepo                            elrepo-kernel
{% endcodeblock %}

### 安装最新版本内核

{% codeblock lang:shell %}
[root@wing-centos ~]# yum --enablerepo=elrepo-kernel install kernel-ml -y
{% endcodeblock %}

### 设置默认启动内核

查看系统上的所有可用内核
{% codeblock lang:shell %}
[root@wing-centos ~]# sudo awk -F\' '$1=="menuentry " {print i++ " : " $2}' /etc/grub2.cfg
0 : CentOS Linux (5.9.11-1.el7.elrepo.x86_64) 7 (Core)
1 : CentOS Linux (3.10.0-1127.19.1.el7.x86_64) 7 (Core)
2 : CentOS Linux (3.10.0-1062.12.1.el7.x86_64) 7 (Core)
3 : CentOS Linux (3.10.0-957.el7.x86_64) 7 (Core)
4 : CentOS Linux (0-rescue-225561433f8f439c95e2b3df836453de) 7 (Core)
{% endcodeblock %}

设置新安装的内容为默认选项
{% codeblock lang:shell %}
[root@wing-centos ~]# grub2-set-default 0
{% endcodeblock %}

生成grub配置文件并重启
{% codeblock lang:shell %}
[root@wing-centos ~]# grub2-mkconfig -o /boot/grub2/grub.cfg
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-5.9.11-1.el7.elrepo.x86_64
Found initrd image: /boot/initramfs-5.9.11-1.el7.elrepo.x86_64.img
Found linux image: /boot/vmlinuz-3.10.0-1127.19.1.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-1127.19.1.el7.x86_64.img
Found linux image: /boot/vmlinuz-3.10.0-1062.12.1.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-1062.12.1.el7.x86_64.img
Found linux image: /boot/vmlinuz-3.10.0-957.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-957.el7.x86_64.img
Found linux image: /boot/vmlinuz-0-rescue-225561433f8f439c95e2b3df836453de
Found initrd image: /boot/initramfs-0-rescue-225561433f8f439c95e2b3df836453de.img
done
[root@wing-centos ~]# reboot
done{% endcodeblock %}

### 验证新版本

{% codeblock lang:shell %}
[root@wing-centos ~]# uname -r
5.9.11-1.el7.elrepo.x86_64
done{% endcodeblock %}

### 删除旧内核

查看系统中全部的内核
{% codeblock lang:shell %}
[root@wing-centos ~]# rpm -qa | grep kernel
abrt-addon-kerneloops-2.1.11-57.el7.centos.x86_64
kernel-tools-libs-3.10.0-1127.19.1.el7.x86_64
kernel-3.10.0-1062.12.1.el7.x86_64
kernel-ml-5.9.11-1.el7.elrepo.x86_64
kernel-headers-3.10.0-1127.19.1.el7.x86_64
kernel-tools-3.10.0-1127.19.1.el7.x86_64
kernel-3.10.0-1127.19.1.el7.x86_64
kernel-3.10.0-957.el7.x86_64
{% endcodeblock %}

删除内核
{% codeblock lang:shell %}
[root@wing-centos ~]# yum remove -y kernel-3.10.0-1062.12.1.el7.x86_64
[root@wing-centos ~]# yum remove -y kernel-3.10.0-1127.19.1.el7.x86_64
[root@wing-centos ~]# yum remove -y kernel-3.10.0-957.el7.x86_64
{% endcodeblock %}

> 参考：https://www.cnblogs.com/xzkzzz/p/9627658.html