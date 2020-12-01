---
title: Linux挂载硬盘
date: 2020-11-30 17:07:02
tags: Linux
categories: Linux
---

### 查询磁盘信息

{% codeblock lang:shell %}
[root@wing-centos ~]# fdisk -l

Disk /dev/vda: 42.9 GB, 42949672960 bytes, 83886080 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000ee330

   Device Boot      Start         End      Blocks   Id  System
/dev/vda1            2048     8390655     4194304   82  Linux swap / Solaris
/dev/vda2   *     8390656    83886046    37747695+  83  Linux

Disk /dev/vdb: 1073.7 GB, 1073741824000 bytes, 2097152000 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
{% endcodeblock %}

### 挂载硬盘

现在需要挂载/dev/vdb这个1TB的磁盘，先创建一个名为data的目录
{% codeblock lang:shell %}
[root@wing-centos /]# mkdir data
{% endcodeblock %}

然后将/dev/vdb挂载到data
{% codeblock lang:shell %}
[root@wing-centos /]# mount /dev/vdb data
{% endcodeblock %}

验证一下结果
{% codeblock lang:shell %}
[root@wing-centos data]# df -Th
Filesystem     Type      Size  Used Avail Use% Mounted on
devtmpfs       devtmpfs  7.9G     0  7.9G   0% /dev
tmpfs          tmpfs     7.9G     0  7.9G   0% /dev/shm
tmpfs          tmpfs     7.9G  8.6M  7.9G   1% /run
tmpfs          tmpfs     7.9G     0  7.9G   0% /sys/fs/cgroup
/dev/vda2      xfs        36G  2.7G   34G   8% /
tmpfs          tmpfs     1.6G     0  1.6G   0% /run/user/0
/dev/vdb       ext4      985G  1.6G  933G   1% /data
{% endcodeblock %}

### 开机自动挂载

修改/etc/fstab文件
{% codeblock lang:shell %}
[root@wing-centos data]# vim /etc/fstab
{% endcodeblock %}

在文件最后一行添加记录

![image info](./images/Linux挂载硬盘_image1.png)

重启主机，验证结果
{% codeblock lang:shell %}
[root@wing-centos ~]# df -Th
Filesystem     Type      Size  Used Avail Use% Mounted on
devtmpfs       devtmpfs  7.9G     0  7.9G   0% /dev
tmpfs          tmpfs     7.9G     0  7.9G   0% /dev/shm
tmpfs          tmpfs     7.9G  8.6M  7.9G   1% /run
tmpfs          tmpfs     7.9G     0  7.9G   0% /sys/fs/cgroup
/dev/vda2      xfs        36G  2.7G   34G   8% /
/dev/vdb       ext4      985G  1.6G  933G   1% /data
tmpfs          tmpfs     1.6G     0  1.6G   0% /run/user/0{% endcodeblock %}
