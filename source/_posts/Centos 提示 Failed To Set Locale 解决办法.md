---
layout: Linux
title: Centos提示failed to set locale, defaulting to C
date: 2020-12-20 00:02:32
tags: Linux
categories: Linux
---


### 解决方法

执行以下命令

``` shell
echo "export LC_ALL=en_US.UTF-8" >> /etc/profile

source /etc/profile
```

