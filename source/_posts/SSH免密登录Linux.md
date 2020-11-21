---
title: Windows 10 SSH 免密登录Linux
date: 2020-11-21 10:34:59
tags: Linux
categories : Linux
---

### 生成SSH密钥
首先，打开PowerShell，输入ssh-keygen命令，默认一直按回车就可以。完成后，会生成一对私钥和公钥文件(id_rsa、id_rsa.pub)，存放在 %USERPROFILE%/.ssh/ 目录里面。后面要使用的公钥文件为<strong>%USERPROFILE%/.ssh/id_rsa.pub</strong>。

{% codeblock lang:bash %}
PS C:\Users\Wing> ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\Wing/.ssh/id_rsa):
Created directory 'C:\Users\Wing/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\Wing/.ssh/id_rsa.
Your public key has been saved in C:\Users\Wing/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:/mjkrJOQbRzCAwlSPYVBNcuxntm/Ms5/MMC15dCRrMc Wing@Wing-Win10-VM-01
The key's randomart image is:
+---[RSA 2048]----+
|oo.+o==    o.o   |
|. o +. =  o =    |
|   o .+. . B     |
|    +..+o o E    |
|     *+.S. .     |
|    o +...o      |
|     o =. .o     |
|      o.*o ..    |
|      .=+++.     |
+----[SHA256]-----+
PS C:\Users\Wing>
{% endcodeblock %}

### 将SSH密钥复制到远程Linux设备
接下来，使用以下命令将公钥复制到Linux设备
{% codeblock lang:bash %}
type $env:USERPROFILE\.ssh\id_rsa.pub | ssh {IP-ADDRESS-OR-FQDN} "cat >> .ssh/authorized_keys"
{% endcodeblock %}

### 测试无密码连接Linux
最后，验证SSH无法密码连接Linux
{% codeblock lang:bash %}
PS C:\Users\Wing> ssh root@141.146.246.173
Last failed login: Sat Nov 21 23:35:54 CST 2020 from 200.199.26.15 on ssh:notty
There were 1503 failed login attempts since the last successful login.
Last login: Mon Nov 16 11:47:50 2020 from 125.137.54.234
[root@43-c58578794-0048-0115660 ~]# who
root     pts/1        2020-11-21 23:38 (120.235.9.96)
[root@43-c58578794-0048-0115660 ~]#
{% endcodeblock %}