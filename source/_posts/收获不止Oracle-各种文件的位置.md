---
title: 收获不止Oracle-各种文件的位置
date: 2018-03-26 17:20:48
tags: Oracle
categories: 收获不止Oracle
---

#### 参数文件位置

show parameter spfile;


![show parameter spfile](/images/shouhuo/9.png)

<!--more-->

#### 控制文件位置

show parameter control;


![控制文件](/images/shouhuo/10.png)

#### 数据文件位置

select file_name from dba_data_files;


![数据文件](/images/shouhuo/11.png)

#### 日志文件位置：

select group#,member from v$logfile;


![日志文件](/images/shouhuo/12.png)

不知道为什么GROUP#那一列格式化后就变成了\#\#\#\#\#\#，本身是1,2,3。

#### 归档文件位置

show parameter recovery


![归档文件](/images/shouhuo/13.png)


#### 告警日志文件（位于bdump目录下，以alert打头的文件）

show parameter dump

![告警日志](/images/shouhuo/14.png)