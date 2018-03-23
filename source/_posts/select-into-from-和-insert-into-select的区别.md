---
title: select into from 和 insert into select的区别
date: 2018-03-14 10:28:59
tags: oracle
categories: 收获不止Oracle
---


### select into from 和 insert into select区别

两者都是用来复制表数据。


##### INSERT INTO SELECT语句
    语句形式为：INSERT INTO Table2（field1, field2,...） select value1, value2,... from Table1
要求目标表Table2必须存在。


###### SELECT INTO FROM语句

      语句形式为：SELECT vale1, value2 into Table2 from Table1

要求目标表Table2不存在，因为在插入时会自动创建表Table2，并将Table1中指定字段数据复制到Table2中。