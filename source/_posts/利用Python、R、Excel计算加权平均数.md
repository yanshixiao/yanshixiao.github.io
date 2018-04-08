---
title: 利用Python、R、Excel计算加权平均数
date: 2018-04-08 21:38:45
tags: Python R Excel 加权平均数
categories: 商务与经济统计
---

### Python

```Python
import numpy as np

a = (70, 80, 60)

np.mean(a)  # 平均值

r = np.average(a, weights=[3, 3, 4])  # 加权平均值

print(r)
```

![Python](/images/data_analyze/1.png)

### R

```Python
a <- c(70,80,60)
mean(a)  #平均值

wt <- c(3,3,4)
weighted.mean(a,wt) #加权平均值
```

![R](/images/data_analyze/2.png)

### Excel
如图，多个学生成绩，在加权总和先计算出每行和数据和对应权值的乘积之和，再作除法即可得出结果。

利用`SUMPRODUCT()`函数，接收两个参数，一个权值序列，一个真实数据序列。

![计算乘积和](/images/data_analyze/3.png)

![计算加权平均数](/images/data_analyze/4.png)


