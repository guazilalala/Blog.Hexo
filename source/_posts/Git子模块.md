---
title: Git子模块
date: 2020-11-23 23:03:30
tags: Git
categories : Git
---

### 添加子模块

{% codeblock %}
git submodule add -b [分支名称] --name [子模块名称] [子模块git地址] [子模块文件夹路径]
{% endcodeblock %}

通过命令添加子模块后，根目录会多一个文件.gtmodules，内容如下：
{% codeblock %}
[submodule "themes/next"]
    path = themes/next
    url =  https://github.com/next-theme/hexo-theme-next.git
{% endcodeblock %}
每添加一个子模块就会新增一条记录。

### 删除子模块

在版本控制中删除子模块：
{% codeblock %}
git rm -r themes/next
{% endcodeblock %}

从.gitmodules文件中删除相关记录
{% codeblock %}
[submodule "themes/next"]
    path = themes/next
    url = https://github.com/next-theme/hexo-theme-next.git
{% endcodeblock %}

从.git/config中删除相关记录
{% codeblock %}
[submodule "themes/next"]
    url = https://github.com/next-theme/hexo-theme-next.git
    active = true
{% endcodeblock %}

删除.git目录里面的模块
{% codeblock lang:bash %}
 rm -rf .git/modules/themes/next/
{% endcodeblock %}
