---
layout: post
title: "Xcode6为什么干掉pch（Precompile Prefix Header）&amp;如何添加pch文件"
date: 2014-11-08 10:11:23 +0800
comments: true
categories: iOS开发
keywords: iOS, Xcode6, pch, Precompile Prefix Header, pch文件, iOSDevTip iOS开发, gcd, cocoapods, 队列, 个人博客, 刚刚在线
description: "Xcode6为什么干掉pch（Precompile Prefix Header）&amp;如何添加pch文件" 

---
一直在用xcode6开发，但项目都是在xcode5上创建的，所以一直没注意到，xcode6竟然干掉pch文件了。

<div class="entry-content"><h3>为什么xcode6没有自动创建pch文件呢？</h3>

简单地看：我们在写项目的时候，大部分宏定义，头文件导入都在这里，Xcode6去掉Precompile Prefix Header的主要原因可能在于Prefix Header大大的增加了Build的时间。没有了Prefix Header之后就要通过手动@import来手动导入头文件了，在失去了编程便利性的同时也降低了Build的时间。
具体原因

StackOverFlow上讨论的已经比较清晰了

<a href="http://stackoverflow.com/questions/24158648/why-isnt-projectname-prefix-pch-created-automatically-in-xcode-6"target="_blank"title="StackOverFlow:为什么xcode6没有自动创建pch文件呢？">StackOverFlow:为什么xcode6没有自动创建pch文件呢？</a>  





<div class="entry-content"><h3>如何在Xcode6中添加pch（Precompile Prefix Header）？</h3>


1，Command+N，打开新建文件窗口：ios->other->PCH file，创建一个pch文件：“工程名-Prefix.pch”：

<img src="http://images.90159.com/11/pch1.png">

2，将building setting中的precompile header选项的路径添加“$(SRCROOT)/项目名称/pch文件名”（例如：$(SRCROOT)/LotteryFive/LotteryFive-Prefix.pch）

<img src="http://images.90159.com/11/pch2.png">

可以了，编译一下程序，如果有错误检查一下添加的路径是否正确。

3，将Precompile Prefix Header为YES，预编译后的pch文件会被缓存起来，可以提高编译速度

<img src="http://images.90159.com/11/pch3.png">




我的微信公众号 iOS开发 ： 
	
	iOSDevTip

✎最新iOS、iPhone资讯，iOS开发者、swift开发、果粉聚集，参与技术讨论，整理开发技巧，分享创业经验！享受生活、热爱编程！