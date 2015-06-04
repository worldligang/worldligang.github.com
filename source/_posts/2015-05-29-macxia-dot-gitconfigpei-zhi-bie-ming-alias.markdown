---
layout: post
title: "mac下.gitconfig配置别名alias"
date: 2015-05-29 13:01:00 +0800
comments: true
categories: iOS开发
keywords: git, alias, 缩写,  .gitconfig, mac, 别名, mac git, iOS开发, cocoapods, 个人博客, 刚刚在线
description: "iOS协议里面可以定义成员变量和属性吗？"

---

在多人协作开发时，一般用git来进行代码管理。git有一些命令如：git pull 、 git push等等，这些命令可以设置alias，也就是缩写。如：git pull 是 git pl， git push 是 git ps。可以根据个人习惯进行修改。

修改别名alias步骤如下：

####1,进入根目录

	$ cd

####2,查看根目录下面的文件

	$ ls -la

查看一下有没有.gitconfig文件。	

####3，编辑.gitconfig

	$ vi .gitconfig
	
添加如下：

	[alias]
    co = checkout
    ci = commit
    st = status
    pl = pull
    ps = push
    dt = difftool
    l = log --stat
    cp = cherry-pick
    ca = commit -a
    b = branch
    
具体可以根据个人习惯进行添加修改。

>关注公众号iOS开发：iOSDevTip
