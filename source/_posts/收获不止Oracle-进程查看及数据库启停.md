---
title: 收获不止Oracle-进程查看及数据库启停
date: 2018-03-26 15:43:54
tags: Oracle 进程 启停
categories: 收获不止Oracle
---

#### 进程的查看

show parameter instance_name查看实例名称

![查看实例名称](/images/shouhuo/4.png)

<!--more-->

##### 修改归档模式

ARCH进程，有的数据库只是为了测试，不需要将日志归档，所以为了提高效率并未开启归档。

![查看是否开启了归档](/images/shouhuo/5.png)

更改归档模式需要重启数据库。首先将数据库置于mount状态，输入alter database archivelog，然后alter database open。如果是从归档修改为非归档，archivelog 变为 noarchivelog。

![修改数据库归档模式](/images/shouhuo/6.png)

---

#### 数据库的启停
##### 启动
数据库启动分为三个阶段：**nomount**,**mount**和**open**。启动时可以输入startup启动，也可以使用startup nomount、startup mount、alter database open分别启动到三个阶段。

1. **startup nomount**阶段。这个阶段主要是去读取参数文件（pfile和spfile），根据文件中的配置分配内存资源并启动相应后台进程，也就是创建实例。
2. **startup mount**阶段。实例已经创建，根据参数文件指定的控制文件名称和地址去查找控制文件。找到就锁定该文件，控制文件里记录了数据库中数据文件、日志文件、检查点等非常重要的信息。锁定控制文件成功就表示数据库mount成功，为实例和数据库之间桥梁的搭建打下了基础。
3. **alter database open**阶段。根据控制文件记录的信息，定位到数据库文件、日志文件等，从而正式开通了实例和数据库之间的桥梁。

![三个启动阶段](/images/shouhuo/7.png)

##### 关闭

使用shutdown immediate命令。

![关闭数据库](/images/shouhuo/8.png)