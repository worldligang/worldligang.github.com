---
layout: post
title: "iOS开发之沙盒机制（SandBox)"
date: 2015-07-20 00:36:13 +0800
comments: true
categories: iOS开发
keywords: 沙盒机制, 读取文件, 沙盒, iOS开发, 刚刚在线
description: iOS APP可以在自己的沙盒里读写文件，但是，不可以访问其他APP的沙盒。每一个APP都是一个信息孤岛，相互是不可以进行通信的。

---

iOS APP可以在自己的沙盒里读写文件，但是，不可以访问其他APP的沙盒。每一个APP都是一个信息孤岛，相互是不可以进行通信的，唯独可以通过[URL Scheme](http://www.superqq.com/blog/2015/07/14/xcode6-dot-4zhu-ce-url-schemebu-zou-xiang-jie/)。沙盒里面的文件可以是照片、声音文件、文本、属性列表等。

##沙盒机制简介

**沙盒简述：**

1. 每一个APP都有一个存储空间，就是沙盒。
2. APP之间不能相互通信。
3. 沙盒根目录结构：Documents、Library、temp。

简述一下Documents、Library、tmp的区别：

1. **Documents：**用于存储用户数据，iTunes备份和恢复的时候会包括此目录，所以，苹果建议将程序中建立的或在程序中浏览到的文件数据保存在该目录下。
2. **Library：**包含两个子目录：Caches 和 Preferences。Caches用来存放用户需要换成的文件。Preferences是APP的偏好设置，可以通过NSUserDefaults来读取和设置。
3. **tmp：** 用于存放临时文件，这个可以放一些当APP退出后不再需要的文件。


##打开模拟器沙盒方法

**方法一：前往文件夹打开**

	/Users/ligang/Library/Application Support/iPhone Simulator/ 

直接在Finder->前往->前往文件夹，输入上面的内容即可。记住将ligang换成你的电脑用户名。

**方法二：通过Finder一步步查找**

模拟器上的APP的沙盒实在用户目录下的资源库里面，但是资源库是隐藏文件夹。所以查看沙盒之前，现将隐藏文件夹显示出来。显示隐藏文件的命令：

	defaults write com.apple.finder AppleShowAllFiles -bool true

用心的童鞋会发现，隐藏隐藏文件的命令：

	defaults write com.apple.finder AppleShowAllFiles -bool false
	
查找步骤请看图：

![sandbox](http://7xjrlb.com1.z0.glb.clouddn.com/sandbox.jpg)

关于沙盒机制（SandBox)的介绍大概就是这样，以后面试的时候万一碰到你就可以这么回答。

