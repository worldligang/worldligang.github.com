---
layout: post
title: "报错：process launch failed：failed to get the task for process 1285（解决方案）"
date: 2015-07-21 00:16:31 +0800
comments: true
categories: iOS开发
keywords: 沙盒机制, 读取文件, 沙盒, iOS开发, 刚刚在线
description: 初接触iOS开发的童鞋估计会被真机调试搞的稀里糊涂的。在模拟器上运行什么问题没有，真机调试就会碰到各种各样的问题。


---

##你遇到的问题别人也遇到过

初接触iOS开发的童鞋估计会被真机调试搞的稀里糊涂的。在模拟器上运行什么问题没有，真机调试就会碰到各种各样的问题。

不用担心。记住：你碰到的问题别人肯定都已经碰到过了。我们不是有百度和谷歌嘛，把你的问题描述出来，一定有解决方案的。还可以在[刚刚在线站内搜索](http://zhannei.superqq.com/)里搜索，说不定也能帮你解决不少问题呢。

我会经常把自己开发过程中碰到的调试问题，一一总结出来。一来是帮助自己在下一次碰到同样问题时，可以游刃有余。二来是帮助大家再碰到类似问题的时候，有一个参考的作用。


##failed to get the task for process 1285

这一次碰到的问题是这样的：模拟器上运行正常。我在iPhone 6上运行，程序编译正常，但是跑起来之后，大概一两秒的时间，就闪退（代码是没有问题的）。这个时候，Xcode6.4弹出这样的提示：
	
![fixbug1285](http://7xjrlb.com1.z0.glb.clouddn.com/fixbug12850.jpg)

	Could not launch “iOSStrongDemo”
	process launch failed: failed to get the task for process 1285

##错误分析

我看了一下Build Settings里的Code Signing配置，发现Provisioning Profile选择的是XC Ad Hoc: *。如下图所示：

![fixbug12851](http://7xjrlb.com1.z0.glb.clouddn.com/fixbug12851.jpg)

Ad Hoc是用来实现发布前的用户测试的，也就是通过Ad Hoc将未发布的App给其他人提前使用。显然这是不对的，我们是要真机调试。

##解决方案

解决方法很简单，将Provisioning Profile换成用开发证书创建的Provisioning Profile即可。