---
title: 收获不止Oracle-体系结构
date: 2018-03-13 09:39:38
tags: Oracle
categories: 收获不止Oracle
---

#### 体系结构
啥也不说，先贴个图

![image](../images/shouhuo/3.png)

<!--more-->

##### 通过执行计划了解缓存区
执行两次，第二次有缓存，对比执行时间及统计信息。

第一次：

```
SQL> create table t as select * from all_objects;                                                               
表已创建。                                                                       
SQL> desc t;                                                                                     
 名称                                      是否为空? 类型                                                
 ----------------------------------------- -------- ----------------------------                 
 OWNER                                     NOT NULL VARCHAR2(30)                                 
 OBJECT_NAME                               NOT NULL VARCHAR2(30)                                 
 SUBOBJECT_NAME                                     VARCHAR2(30)                                 
 OBJECT_ID                                 NOT NULL NUMBER                                       
 DATA_OBJECT_ID                                     NUMBER                                       
 OBJECT_TYPE                                        VARCHAR2(19)                                 
 CREATED                                   NOT NULL DATE                                         
 LAST_DDL_TIME                             NOT NULL DATE                                         
 TIMESTAMP                                          VARCHAR2(19)                                 
 STATUS                                             VARCHAR2(7)                                  
 TEMPORARY                                          VARCHAR2(1)                                  
 GENERATED                                          VARCHAR2(1)                                  
 SECONDARY                                          VARCHAR2(1)                                  
 NAMESPACE                                 NOT NULL NUMBER                                       
 EDITION_NAME                                       VARCHAR2(30)                                 
                                                                                                 
SQL> create index idx_object_id on t(object_id);                                                 
                                                                                                 
索引已创建。                                                                                           
                                                                                                 
SQL> set autotrace on                                                                            
SQL> set linesize 1000                                                                           
SQL> set timing on                                                                               
SQL> select object_name from t where object_id=29;                                               
                                                                                                 
OBJECT_NAME                                                                                      
------------------------------                                                                   
C_COBJ#                                                                                          
                                                                                                 
已用时间:  00: 00: 00.09                                                                             
                                                                                                 
执行计划                                                                                             
----------------------------------------------------------                                       
Plan hash value: 2041828949                                                                      
                                                                                                 
---------------------------------------------------------------------------------------------    
| Id  | Operation                   | Name          | Rows  | Bytes | Cost (%CPU)| Time     |    
---------------------------------------------------------------------------------------------    
|   0 | SELECT STATEMENT            |               |     1 |    30 |     2   (0)| 00:00:01 |    
|   1 |  TABLE ACCESS BY INDEX ROWID| T             |     1 |    30 |     2   (0)| 00:00:01 |    
|*  2 |   INDEX RANGE SCAN          | IDX_OBJECT_ID |     1 |       |     1   (0)| 00:00:01 |    
---------------------------------------------------------------------------------------------    
                                                                                                 
Predicate Information (identified by operation id):                                              
---------------------------------------------------                                              
                                                                                                 
   2 - access("OBJECT_ID"=29)                                                                    
                                                                                                 
Note                                                                                             
-----                                                                                            
   - dynamic sampling used for this statement (level=2)                                          
                                                                                                 
                                                                                                 
统计信息                                                                                             
----------------------------------------------------------                                       
         52  recursive calls                                                                     
          0  db block gets                                                                       
         79  consistent gets                                                                     
          1  physical reads                                                                      
          0  redo size                                                                           
        535  bytes sent via SQL*Net to client                                                    
        520  bytes received via SQL*Net from client                                              
          2  SQL*Net roundtrips to/from client                                                   
          0  sorts (memory)                                                                      
          0  sorts (disk)                                                                        
          1  rows processed                               
SQL>                                    
```

第二次：
```
SQL> select object_name from t where object_id=29;                                               
                                                                                                 
OBJECT_NAME                                                                                      
------------------------------                                                                   
C_COBJ#                                                                                          
                                                                                                 
已用时间:  00: 00: 00.01                                                                             
                                                                                                 
执行计划                                                                                             
----------------------------------------------------------                                       
Plan hash value: 2041828949                                                                      
                                                                                                 
---------------------------------------------------------------------------------------------    
| Id  | Operation                   | Name          | Rows  | Bytes | Cost (%CPU)| Time     |    
---------------------------------------------------------------------------------------------    
|   0 | SELECT STATEMENT            |               |     1 |    30 |     2   (0)| 00:00:01 |    
|   1 |  TABLE ACCESS BY INDEX ROWID| T             |     1 |    30 |     2   (0)| 00:00:01 |    
|*  2 |   INDEX RANGE SCAN          | IDX_OBJECT_ID |     1 |       |     1   (0)| 00:00:01 |    
---------------------------------------------------------------------------------------------    
                                                                                                 
Predicate Information (identified by operation id):                                              
---------------------------------------------------                                              
                                                                                                 
   2 - access("OBJECT_ID"=29)                                                                    
                                                                                                 
Note                                                                                             
-----                                                                                            
   - dynamic sampling used for this statement (level=2)                                          
                                                                                                 
                                                                                                 
统计信息                                                                                             
----------------------------------------------------------                                       
          0  recursive calls                                                                     
          0  db block gets                                                                       
          4  consistent gets                                                                     
          0  physical reads                                                                      
          0  redo size                                                                           
        535  bytes sent via SQL*Net to client                                                    
        520  bytes received via SQL*Net from client                                              
          2  SQL*Net roundtrips to/from client                                                   
          0  sorts (memory)                                                                      
          0  sorts (disk)                                                                        
          1  rows processed                                                                      
                                                                                                 
SQL>                                                                                             
```

强制不用索引读，比较不同方案的COST，验证是否靠谱。利用`HIST`写法。

原来的sql：
    
    select object_name from t where object_id=29;
    
不走索引：
    
    select /*+full(t)*/ object_name from t where object_id=29;
    
> 注意：要执行两遍，消除共享池解析、减少甚至消除物理读和递归调用。

结果如下：
```
SQL> select /*+full(t)*/ object_name from t where object_id=29;

OBJECT_NAME
------------------------------
C_COBJ#

已用时间:  00: 00: 00.00

执行计划
----------------------------------------------------------
Plan hash value: 1601196873

--------------------------------------------------------------------------
| Id  | Operation         | Name | Rows  | Bytes | Cost (%CPU)| Time     |
--------------------------------------------------------------------------
|   0 | SELECT STATEMENT  |      |     1 |    30 |   280   (1)| 00:00:04 |
|*  1 |  TABLE ACCESS FULL| T    |     1 |    30 |   280   (1)| 00:00:04 |
--------------------------------------------------------------------------

Predicate Information (identified by operation id):
---------------------------------------------------

   1 - filter("OBJECT_ID"=29)

Note
-----
   - dynamic sampling used for this statement (level=2)


统计信息
----------------------------------------------------------
          0  recursive calls
          0  db block gets
       1026  consistent gets
          0  physical reads
          0  redo size
        535  bytes sent via SQL*Net to client
        520  bytes received via SQL*Net from client
          2  SQL*Net roundtrips to/from client
          0  sorts (memory)
          0  sorts (disk)
          1  rows processed
```

COST指CPU耗费：Oracle估计的该步骤的执行成本，用于说明SQL执行的代价，理论上越小越好（该值可能与实际有出入）

TIME指Oracle估计的当前操作所需的时间

-----
##### 各个后台进程的作用

更新时，COMMIT无法左右数据何时从数据缓冲区刷入到数据区。（当前SESSION可以立即查看到数据修改）缓冲区的数据积累到一定程度，再批量刷入到磁盘中。DBWR将缓冲区数据写入磁盘不是由commit决定的，而是由ckpt进程决定的，在此之前，LGWR会将数据写入日志文件，避免断电丢失数据。


PMON：Processes Monitor，进程监视器，执行某些更新语句，未提交时进程崩溃了，PMON会自动回滚。除此之外还可以干预后台进程，遇到LGWR进程失败这样的严重问题，会终止实例，防止数据错乱。

LCKn仅使用于RAC数据库，最多可有10个进程，用于实例间的封锁。

RECO：用于分布式数据库的恢复，全称Distributed Database Recovery。

CKPT：由Oracle的FAST_START_MTTR_TARGET参数控制，用于触发DBWR从数据缓冲区中写出数据到磁盘。CKPT执行越频繁，DBWR写出越频繁，DBWR写出越频繁越不能显示批量特性，但是数据库异常恢复的时候会越迅速。

DBWR：负责吧数据缓冲区写出到磁盘里，该进程和CKPT相辅相成。因为是CKPT促成DBWR去写数据的。不过DBWR也和LGWR相关，因为DBWR想将数据缓冲区数据写到磁盘的时候，必须通知LGWR先完成日志缓冲区写到磁盘的动作后，方可开工。

LGWR：把日志缓冲区的数据从内存写到磁盘的REDO文件里。完成数据库对象创建、更新数据等操作过程的记录。

>LGWR工作相当繁重，记录的的日志顺序也很重要，多进程难以保证顺序，所以LGWR只能采用单进程。LGWR执行的规则如下：
    
    1. 每隔三秒钟，LGWR运行一次。
    2.任何COMMIT触发LGWR运行一次。
    3.DBWR要把数据从数据缓冲写到磁盘，触发LGWR运行一次。
    4.日志缓冲区满三分之一或记录满1MB，触发LGWR运行一次。
    5.联机日志文件切换也触发LGWR。
    
ARCH：作用是在LGWR写日志写到需要覆盖重写的时候，触发ARCH进程去转义日志文件，复制出去形成归档日志文件，以免日志丢失。