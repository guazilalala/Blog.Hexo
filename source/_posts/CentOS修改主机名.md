---
title: CentOS修改主机名
date: 2020-11-28 11:13:08
tags: Linux
categories: Linux
---

### 查看当前主机名

{% codeblock %}
[root@43-c58578794-0048-0115660 ~]# hostname
43-c58578794-0048-0115660.novalocal
{% endcodeblock %}

### 设置主机名

{% codeblock %}
[root@43-c58578794-0048-0115660 ~]# hostnamectl set-hostname Wing-CentOS
[root@43-c58578794-0048-0115660 ~]# hostname
wing-centos
{% endcodeblock %}
