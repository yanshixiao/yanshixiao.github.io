---
title: sublime text3 便携版制作
date: 2018-02-25 19:09:44
tags: sublime text
categories: 软件
---

想要优雅的使用sublime text,插件是必不可少的，而插件的备份闲的尤为重要。只需要将`packages`(Preferences > Browse Packages)中内容拷贝一份，其对应着一个Data目录，exe安装文件版本默认位置在  
`C:\Users\Adminstrator\AppData\Roaming\Data`,   
而zip包解压安装后Data目录就在sublimetext根目录\Data\Packages下。
<!--more-->
#### 破解

网上一抓一大把

#### 安装package control

[传送门](https://packagecontrol.io/installation)
sublime中`ctrl` + ` 调出控制台，将上面网页中的代码粘贴进去回车，等待，重启就ok。

#### 安装插件
**方式一：直接安装**
> 直接下载安装包解压缩至Packages目录，入口在菜单 > preferences > browse packages。

**方式二：使用Package Control安装**
> `ctrl` + `shift` + `p` 调出命令面板，输入install，选择Install Package选项回车

#### 一些插件

- 1.WordCount：实时显示当前文件的字数，貌似中文不好使。

- 2.ConvertToUTF8：解决中文乱码问题，ansi格式的可以直接打开正常查看。

- 3.SideBarEnhancements: 增强右键功能。
- 4.SideBarFolders: 管理打开的文件夹。
- 5.Compare Side-By-Side: 文件比较工具。
- 6.BracketHighlighter: 括号匹配高亮。
- 7.PlainTasks: TODO会被识别
- 8.TrailingSpaces: 高亮显示尾部多余的空格
- 9.AdvancedNewFile: 新建文件。
- 10.Emmet: html代码的快速生成。
- 11.SublimeLinter
- 12.SublimeEnhancements
- 13.Git
- 14.Terminal
- 15.Alignment: 对齐代码
- 16.ColorPicker: 颜色选择
- 17.MarkDownEditing: markdown编辑
- 18.SublimeCodeIntel: 代码自动提示
- 19.AutoFileName: 文件路径自动提示
- 20.ColorSublime: 一个主题管理网站的插件，使用时preferences中看不到，需要控制台中查看。

#### 备份
将Data目录整个打包备份，上传云盘。

#### 新电脑使用
下载zip版sublime，解压后，将Data.zip解压至根目录。
