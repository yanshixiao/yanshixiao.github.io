---
title: hexo和github page博客next主题提交远程问题（多端同步编写博客）
date: 2018-02-06 17:20:30
tags: hexo 博客
categories: 软件 

---

查看众多博客，为了多端同步部署博客，无非是本地的source及配置文件等上传到github，可以是之前博客io那个的一个分支，也可以是另一个新的repo，不过，不论如何，themes/next无法git add，主要牵扯到next它自己下面也有个.git，而且当时还是clone的官方的，各种不好使。最终终于找到个好使的。[传送门](http://devtian.me/2015/03/17/blog-sync-solution/)

可能会出现“already exists in the index”的问题，先执行下面的语句就ok：
    
    git rm -r --cached theme/next

<!--more-->
    
然后将修改好的next文件夹备份一下，重新fork一遍官方到自己的github上，然后执行：
    
    git submodule add git@github.com:yanshixiao/hexo-theme-next.git themes/next
    
随后,将备份的内容覆盖next仓库，在theme/nex目录下执行（这步并非必须，反正后面还要把修改提交到io那个仓库的hexo分支）：
    
    git add .
    git commit -m "next settings in fork next"
    git push origin master //提交到fork后next的仓库
    
切回主目录

    git add .
    git commit -m "update next settings in blog source branch"
    git push origin hexo //注意hexo分支
    
>**Note**: git status 显示还是会出现changes note staged for commit: modified: themes/next，不过不影响，继续

以后写文章，只要在根目录下（hexo分支）进行add,commit,push操作，部署时repo仓库地址是在site主站配置文件里配着，没影响。即：资源（工作用）hexo分支，部署（展示用）master分支。hexo d -g

下面是新电脑的操作
    
    git clone --recursive git@github.com:yanshixiao/yanshixiao.github.io.git //clone 主仓库
    cd yanshixiao.github.io/
    git checkout hexo //切换到hexo，以后基本都是基于此分支，master分支用hexo -d
    cd themes/next/
    git submodule init
    git submodule update //获取我的NexT主题的配置
    //接下来的任务主要是配置环境，nodejs安装，hexo等等。以下安装可能不全面
    //先切换到仓库根目录
    npm install -g hexo
    npm install hexo-cli -g
    npm install hexo --save
    npm install hexo-server --save
    npm install hexo-deployer-git --save
    npm install
