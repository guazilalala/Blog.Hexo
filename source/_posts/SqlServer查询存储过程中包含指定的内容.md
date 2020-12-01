---
title: SqlServer查询存储过程中包含指定的内容
date: 2020-12-01 15:12:06
tags: SQL
categories: SQL
---

{% codeblock lang:sql %}
select obj.name procname
     , sc.text  content
from syscomments          sc
    inner join sysobjects obj
        on sc.id = obj.id
where sc.text like '%要查询的内容%';
{% endcodeblock %}