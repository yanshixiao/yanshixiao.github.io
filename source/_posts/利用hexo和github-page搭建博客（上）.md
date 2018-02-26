---
title: 利用hexo和github page搭建博客（上）
date: 2018-02-05 10:15:46
tags:
---

安装NodeJS和Git并配置环境变量

测试node

node -v
npm -v
​   测试git

电脑任一文件夹右键出现Git GUI Here和Git Bash Here
<!--more-->
Github新建项目

项目名必须遵守：用户名.github.io。并且勾选Initialize this repository with a README

在建好的项目右侧有个setting按钮，点击后向下拉至github pages，访问网址，项目可以外网访问。

安装Hexo

选择位置创建文件夹，比如D:\blog，进入目录，输入npm install hexo -g，开始安装Hexo

输入hexo -v，检查hexo是否安装成功

输入hexo init，初始化该文件夹

输入npm install，安装所需组件

输入hexo g，开启服务器，访问网址，一般端口是4000，如果不好使，可能是被占用，ctrl+c</button>停止服务器，接着输入“hexo server -p 端口号”来改变端口号

将Hexo与Github page联系起来

在blog文件夹右键点击Git Bash Here

设置Git的user name和email（如果是第一次的话）

git config --global user.name "yuanshuai"
git config --golbal user.email "17600232953@163.com"
输入 cd~/.ssh，检查是否有.ssh文件夹

输入ssh-keygen -t rsa -C "17600232953@163.com",一直回车，生成密钥，存储位置为用户根路径下的.ssh文件夹

输入eval "$(ssh-agent -s)"，添加密钥到ssh-agent

输入ssh-add ~/.sssh/id_rsa，添加ssh key到ssh-agent

登录Github添加ssh，拷贝id_rsa.pub内容

配置Deployment

在blog目录下_config.yml文件中，修改repo值

deploy:
    type: get
    repository: git@github.com:用户名.github.io.git
    branch: master
repo值为github项目ssh地址

新建博客

cmd执行：hexo new post "博客名"

生成部署之前，需要安装一个扩展：npm install hexo-deployer-git --save

hexo d -g，生成及部署

修改主题参考如下链接：

传送门

其中有一些问题，下篇详细列举。
