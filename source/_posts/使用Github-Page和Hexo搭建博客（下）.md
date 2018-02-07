---
title: 使用Github Page和Hexo搭建博客（下）
date: 2018-02-06 18:23:28
tags: 博客 Hexo
categories: 软件

---

链接中侧边栏的头像图说是avatar配置在主站conf下，其实查看themes/next/layout/_macro/sidebar.swig, 可以看到读取的是theme.authorimage 而非 config.authorimage。所以其实应该配在主题配置中，或者修改swig中内容。
![](http://ww1.sinaimg.cn/large/006S91wdgy1fo6wum59ktj30hb06w3yv.jpg)
<!--more-->
还有侧边栏的图标
![](http://ww1.sinaimg.cn/large/006S91wdgy1fo6xsam590j306k07u745.jpg)
文中的格式有误，通过观察 还是上述文件
![](http://ww1.sinaimg.cn/large/006S91wdgy1fo6xxiodsbj30hb063aa7.jpg)
可以看出，配置格式应该为：
![](http://ww1.sinaimg.cn/large/006S91wdgy1fo6xyqa366j30bx0580st.jpg)

剩下的就是各种js样式优化网站了。比如页面网格线飘动的效果，鼠标光标替换的效果等等。[传送门](https://segmentfault.com/a/1190000009544924)
