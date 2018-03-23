---
title: 利用Python、R、Excel做频数统计并绘制条形图和饼图
date: 2018-03-07 11:03:41
tags: 数据分析 Python R Excel
categories: 商务与经济统计
---

Excel、Python、R三种方法进行频数分析并画出条形图和饼形图

Excel 利用函数COUNTIF，传入两个参数，第一个为源数据，第二个为分类数据各列一遍（即频数统计的各项）

-----
### Python
##### 频数统计
利用count()函数，具体见下方条形图的代码。
##### 条形图
方法如下：

> matplotlib.pyplot.bar(left, height, width=0.8, bottom=None, hold=None, data=None, **kwargs)

参数说明：

- left: 每一个柱形左侧的X坐标

- height:每一个柱形的高度

- width: 柱形之间的宽度

- bottom: 柱形的Y坐标

- color: 柱形的颜色

<!--more-->

[官网解释](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.bar.html#matplotlib.pyplot.bar)

代码示例：

    from matplotlib import pyplot as plt
    import numpy as np
    
    # 中文乱码的处理
    plt.rcParams['font.sans-serif'] =['Microsoft YaHei']
    plt.rcParams['axes.unicode_minus'] = False
    
    #第一步，取出一张白纸
    fig = plt.figure(1)
    
    #第二步，确定绘图范围，由于只需要画一张图，所以我们将整张白纸作为绘图的范围(111等同于1,1,1 表示一行一列的第一个)
    ax1 = plt.subplot(111)
    
    #第三步，整理我们准备绘制的数据
    data = np.array([15, 20, 18, 25])
    
    #第四步，准备绘制条形图，思考绘制条形图需要确定那些要素
    #1、绘制的条形宽度
    #2、绘制的条形位置(中心)
    #3、条形图的高度（数据值）
    width=0.5
    x_bar = np.arange(4)
    
    #第五步，绘制条形图的主体，条形图实质上就是一系列的矩形元素，我们通过plt.bar函数来绘制条形图
    rect = ax1.bar(left=x_bar, height=data, width=width, color="lightblue")
    
    #第六步，向各条形上添加数据标签
    for rec in rect:
        x=rec.get_x()
        height=rec.get_height()
        ax1.text(x+0.1,1.02*height,str(height))
    
    #第七步，绘制x，y坐标轴刻度及标签，标题
    ax1.set_xticks(x_bar)
    ax1.set_xticklabels(("first", "second", "third", "fourth"))
    ax1.set_ylabel("sales")
    ax1.set_title("The Sales in 2016")
    ax1.grid(True)
    ax1.set_ylim(0, 28)
    plt.show()


其中第五步创建矩形元素中，
left为x轴的位置序列，一般采用arange函数产生一个序列；height为y轴的数值序列，也就是柱形图的高度，一般就是我们需要展示的数据；width为柱形图的宽度。

或者：

    import random
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    tv_show = ["WoF", "THM", "Jep", "JJ", "OWS"]
    tvs = [random.choice(tv_show) for _ in range(50)]
    # print(tvs)
    # 频数
    WoF_count = tvs.count("WoF")
    THM_count = tvs.count("THM")
    Jep_count = tvs.count("Jep")
    JJ_count = tvs.count("JJ")
    OWS_count = tvs.count("OWS")
    
    # 条形图
    x_bar = np.arange(5)
    data = np.array([WoF_count, THM_count, Jep_count, JJ_count, OWS_count])
    fig = plt.figure()
    ax1 = plt.subplot(111)
    rect = ax1.bar(left=x_bar, width=0.5, height=data, color="lightblue")
    
    ax1.set_xticks(x_bar)
    ax1.set_xticklabels(tv_show)
    plt.xlabel("X")
    plt.ylabel("Y")
    plt.title("bar chart")
    plt.show()
    
画出的图像是这样的
![](http://ww1.sinaimg.cn/large/006S91wdgy1fp4214rt3oj30hs0d50sq.jpg)
    
##### 饼形图
方法及参数：

> pie(x, explode=None, labels=None,  
        colors=('b', 'g', 'r', 'c', 'm', 'y', 'k', 'w'),  
        autopct=None, pctdistance=0.6, shadow=False,  
        labeldistance=1.1, startangle=None, radius=None,  
        counterclock=True, wedgeprops=None, textprops=None,  
        center = (0, 0), frame = False )  
    
参数说明：
- x       (每一块)的比例，如果sum(x) > 1会使用sum(x)归一化
- labels  (每一块)饼图外侧显示的说明文字
- explode (每一块)离开中心距离
- startangle  起始绘制角度,默认图是从x轴正方向逆时针画起,如设定=90则从y轴正方向画起
- shadow  是否阴影
- labeldistance label绘制位置,相对于半径的比例, 如<1则绘制在饼图内侧
- autopct 控制饼图内百分比设置,可以使用format字符串或者format function
- '%1.1f'指小数点前后位数(没有用空格补齐)
- pctdistance 类似于labeldistance,指定autopct的位置刻度
- radius  控制饼图半径

随便以测试数据为例，画风大致是这样的
    
    import numpy as np
    import matplotlib.pyplot as plt
    
    labels=['China','Swiss','USA','UK','Laos','Spain']
    x = [222, 42, 455, 664, 454, 334]
    explode = [0, 0.1, 0, 0, 0, 0] # 0.1 凸出这部分，
    
    fig = plt.figure()
    plt.pie(x, labels=labels, explode=explode, autopct='%1.2f%%')
    plt.title("Pie chart")
    
    plt.show()
    plt.savefig("PieChart.jpg")
    
![](http://ww1.sinaimg.cn/large/006S91wdgy1fp425rzm6mj30hs0d5t99.jpg)
    
-------------
    
### R
利用table()函数，即可直观的看到频数。

举个栗子：
    
    a <- c('A','B','C','A','D','B','E')

    test <- table(a)
    # 提取table()中的元素
    names(test)
    # 提取table()中的频数
    as.numeric(test)
    
    # 转换
    test2 <- as.data.frame(test)
    test2
    # 提取table()中频数为1的数据
    test2[which(test2$Freq==1),]
    # 提取table()中指定元素2的频数
    test2[which(test2$a=='A'),]
    
##### 画条形图：

    library(ggplot2)
    test <- c('A','B','C','D','A','C','C')
    test1 <- table(test)
    test2 <- as.data.frame(test1)
    x<-test2$test
    y<-test2$Freq
    ggplot(data=test2, aes(x=test, y=Freq)) + geom_bar(stat='identity')
    
    
使用汇总好的数据画图

    x <- c('A','B','C','D','E')
    y <- c(13,22,16,31,8)
    df <- data.frame(x = x, y = y)
    ggplot(data = df, mapping = aes(x = x, y = y)) + geom_bar(stat = 'identity')
    
![](http://ww1.sinaimg.cn/large/006S91wdly1fp42d255vaj30cj0a7q2q.jpg)
    
要强调的是如果横轴对应数据非离散型，则须先将其转为因子类型，否则结果会出现"空条"。利用factor()函数。

[geombar官方文档](http://ggplot2.tidyverse.org/reference/geom_bar.html)

##### 饼图：
    x <- c(9, 2, 4, 5)
    labels <- c('A', 'B', 'C', 'D')
    
    # 给文件指定名称
    png(file="ABCD.jpg")
    # 画图
    pie(x, labels, main='pie chart demo', col=rainbow(length(x)), radius = 0.9)
    # 保存
    dev.off()
    
![](http://ww1.sinaimg.cn/large/006S91wdgy1fp42i5mveoj30dc0dcglf.jpg)
  
使用R画饼图的语法如下：  
> pie(x, labels, radius, main, col, clockwise)

以下是使用的参数的描述 -

- x - 是包含饼图中使用的数值的向量。
- labels - 用于描述切片的标签。
- radius - 用来表示饼图圆的半径(-1和+1之间的值)。
- main - 用来表示图表的标题。
- col - 表示调色板。
- clockwise - 是一个逻辑值，指示片是顺时针还是逆时针绘制。