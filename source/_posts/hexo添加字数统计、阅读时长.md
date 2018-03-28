---
title: hexo添加字数统计、阅读时长
date: 2018-03-28 11:49:55
tags: 博客 hexo
categories: hexo
---

字数统计和阅读时长next主题已经集成。修改配置文件。

![](/images/hexo/1.png)

hexo server启动本地预览可能发现并没有生效。一般是因为没装插件。使用如下命令（最好以管理员打开cmd）：

`npm install hexo-wordcount --save`

安装完成后，使用`hexo --debug`查看是否安装成功。

还有一件事儿，此时页面上统计是光秃秃的数字，没有单位。不多说，加上。

修改主题目录下\layout\_macro\post.swig。添加“字”和“分钟”。

![](/images/hexo/2.png)