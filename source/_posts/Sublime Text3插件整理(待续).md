---
title: Sublime Text3插件整理(待续)
date: 2018-02-03 00:25:08
tags: sublime
categories: 软件
description: Sublime Text 3插件
---
# Sublime Text 3插件整理

## MarkDown Editing
    markdown编辑查看，适当的颜色高亮和其他功能。 
<!--more-->
## ColorPicker
    使用ctrl+shift+c快捷键调用颜色选择器。

## SublimeREPL
    这可能是对程序员很有用的插件。SublimeREPL 允许你在 Sublime Text 中运行各种语言（NodeJS ， Python，Ruby， Scala 和 Haskell 等等）。

## Ctags
    函数跳转，类似ide点击进入函数定义，alt+<-返回原处等。由于是正则匹配实现，要求代码很规范。插件相对来说有些麻烦。

## SublimeLinter
    前端编辑利器，高亮提示用户编写代码中存在的不规范和错误的写法。

## SideBarEnhancements
    右键增强插件。该插件还能让我们自定义快捷键呼出某个浏览器以预览页面。
    安装插件后，点击点击菜单栏的preferences->package setting->side bar->Key Building-User，键入以下代码：
```
    [   
    { "keys": ["ctrl+shift+c"], "command": "copy_path" },
    //chrome
    { "keys": ["f2"], "command": "side_bar_files_open_with",
            "args": {
                "paths": [],
                "application": "浏览器程序路径",
                "extensions":".*"
            }
     }
]
```
## SublimeTmpl
    快速生成文件模板。默认快捷键：
    '''
        ctrl+alt+h html
        ctrl+alt+j javascript
        ctrl+alt+c css
        ctrl+alt+p php
        ctrl+alt+r ruby
        ctrl+alt+shift+p python
    '''
    新建其他类型文件模板TODO

## advancedNewFile
    快速创建文件 ctrl+alt+n  

    若保存.md文件时无法保存到当前文件夹，可安装AdvancedNewFile插件，方法与前面两款插件相同，并且进行设置，同样将default的设置代码复制到User，找到"default_root": "project_folder",，把project_folder改为current，保存。

## ConvertToUTF8
打开文件自动转换为utf8显示

## Bracket Highlighter
    用于匹配括号，引号和html标签。

## ChineseLocalizations
    让sublime汉化的插件

## Emmet
    快速编写html/css。

## Sublime CodeIntel 
    代码自动提示