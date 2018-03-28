---
title: R语言入门
date: 2018-03-23 14:21:20
tags: R语言
categories: R语言
---

##### 运行R
运行R有两种方式：一为命令行模式，即直接打开R.exe文件，进入带有版本信息等的控制台页面。如图

![R控制台](/images/R/R-1.png)
<!--more-->

通常，R代码文件都会有后缀.R，如果创建了一个z.R文件，可以用在控制台source("z.R")执行文件中的代码。

![R控制台](/images/R/R-2.png)

二为批处理模式，R CMD BATCH z.R

![R CMD BATCH](/images/R/R-3.png)
![R 运行结果](/images/R/R-4.png)

##### 一些简单的操作

    >x <- c(1,2,4)

R语言的标准赋值运算符是 <-。

    > x[3]
    [1] 4

R向量的元素索引是从1开始的，而非0。

    > mean(x)
    [1] 2.333333
    > sd(x)
    [1] 1.527525

mean()平均数，sd()标准差

    > mean(Nile)
    [1] 919.35
    > sd(Nile)
    [1] 169.2275
    hist(Nile)

图4，直方图

##### 函数

    # counts the number of odd integers in x
    > oddcount <- function(x) {
    +     k <- 0 # assign 0 to k
    +     for (n in x) {
    +         if(n %%2 ==1) k <- k+1 # %% is the module operator
    +     }
    +     return(k)
    + }
    > oddcount(c(1,3,5))
    [1] 3
    > oddcount(c(1,2,3,7,9))
    [1] 4

计算向量中奇数的数量。

##### 变量作用域

这个没啥可说的，函数内部的变量在外部调用自然会报错。

    > x<-3
    > f <- function(x) return(x+10)
    > f(x)
    [1] 13
    > x
    [1] 3

即使函数改变了向量的值，原本向量也不会变化，因为执行函数时是copy了一份作为局部变量使用的，即 R函数中的形参是局部变量。

##### 默认参数
    > g <- function(x, y=2, z=T) {....}
    > g(12, z=FALSE)

##### R语言中部分数据结构

###### 向量
说到向量，不得不提标量。标量，或单个的数，其实在R中并不存在，R语言把单个的数当做只有一个元素的向量。

###### 字符串
字符串实际上是字符模式（而不是数字模式）的单元素向量。

    > x <- c(5,12,13)
    > x
    [1] 5 12 13
    > length(x)
    [1] 3
    > mode(x)
    [1] "numeric"
    > y <- "abc"
    > y
    [1] "abc"
    > length(y)
    [1] 1
    > mode(y)
    [1] "character"

mode()分析数据类型。

    > u <- paste("abc", "de", "f")
    > u
    [1] "abc de f"
    > v <- strsplit(u, " ") # split the string according to blanks
    > v
    [[1]]
    [1] "abc" "de" "f"

paste()字符串合并, strsplit()分割字符串，第一个参数为待分割的字符串，第二个参数为分割依据。

###### 矩阵
矩阵其实也是向量。不过多了两个附加的属性：行数和列数。

    > m <- rbind(c(1,4), c(2,2))
    > m
         [,1] [,2]
    [1,]    1    4
    [2,]    2    2
    > m %*% c(1,1)
         [,1]
    [1,]    5
    [2,]    4

rbind() 按行绑定。%*%矩阵乘法。

###### 列表
列表中的数据可以是不同的数据类型。

    > x <- list(u=2, v="abc")
    > x
    $u
    [1] 2

    $v
    [1] "abc"

    > x$u
    [1] 2
    > x$v
    [1] "abc"

x$u指的是列表x中的组件u。

    > hist(Nile)
    > hn <- hist(Nile)
    > print(hn)
    $breaks
     [1]  400  500  600  700  800  900 1000 1100 1200 1300 1400

    $counts
     [1]  1  0  5 20 25 19 12 11  6  1

    $density
     [1] 0.0001 0.0000 0.0005 0.0020 0.0025 0.0019 0.0012 0.0011 0.0006 0.0001

    $mids
     [1]  450  550  650  750  850  950 1050 1150 1250 1350

    $xname
    [1] "Nile"

    $equidist
    [1] TRUE

    attr(,"class")
    [1] "histogram"

hist函数的返回值，很显然也是一个列表。

    > str(hn)
    List of 6
     $ breaks  : int [1:11] 400 500 600 700 800 900 1000 1100 1200 1300 ...
     $ counts  : int [1:10] 1 0 5 20 25 19 12 11 6 1
     $ density : num [1:10] 0.0001 0 0.0005 0.002 0.0025 0.0019 0.0012 0.0011 0.0006 0.0001
     $ mids    : num [1:10] 450 550 650 750 850 950 1050 1150 1250 1350
     $ xname   : chr "Nile"
     $ equidist: logi TRUE
     - attr(*, "class")= chr "histogram"

str() 代表structure结构，显示任何R对象的内部结构。和字符串可没啥关系~~~

###### 数据框

    > d <- data.frame(list(kids=c("Jack", "Jill"),ages=c(12,10)))
    > d
      kids ages
    1 Jack   12
    2 Jill   10

###### 类

    > hn <- hist(Nile)
    > summary(hn)
             Length Class  Mode
    breaks   11     -none- numeric
    counts   10     -none- numeric
    density  10     -none- numeric
    mids     10     -none- numeric
    xname     1     -none- character
    equidist  1     -none- logical

str()查看对象时，有属性“class”，表示是个类，如果函数输出结果是一个列表，同时是一个类，可以用summary()函数生成“摘要”。

##### 获取帮助
在线帮助help()。调用help()的快捷方式是用问号（？）。
特殊字符及保留字必须用引号括起来。比如

    > ?"<"

example()函数获取一些示例。