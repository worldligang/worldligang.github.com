---
layout: post
title: "如何查看真机的沙盒（图文教程）"
date: 2015-07-23 00:29:31 +0800
comments: true
categories: iOS开发
keywords: iOS开发, iOS开发者, 真机, iOS开发, 刚刚在线
description: 这样就可以查看模拟器的沙盒内容。在平时开发中，我们应该大部分用的是真机测试。如果需要在沙盒里操作文件，那么我们可以通过观察真机沙盒的内容，来确定文件操作是否正确。

---

在[iOS开发之沙盒机制（SandBox)](http://www.superqq.com/blog/2015/07/20/ioskai-fa-zhi-sha-he-ji-zhi-%28sandbox/)，我们了解到如何查看模拟器的沙盒内容。直接在Finder->前往->前往文件夹里输入：

	/Users/ligang/Library/Application Support/iPhone Simulator/ 
	
这样就可以查看模拟器的沙盒内容。在平时开发中，我们应该大部分用的是真机测试。如果需要在沙盒里操作文件，那么我们可以通过观察真机沙盒的内容，来确定文件操作是否正确。

如果你已经知道如何查看真机的沙盒，请看看其他的文章吧，在这里[刚刚在线站内搜索](zhannei.superqq.com)可以搜索到更多iOS开发相关的文章。接下来，通过图文的方式详细讲解如何查看真机沙盒。有兴趣的同学可以继续往下看。	
	
##查看真机沙盒教程

###打开Devices

在xcode的上部导航栏里，选择window -> Devices

![realsandbox1.jpg](http://7xkkk9.com1.z0.glb.clouddn.com/realsandbox1.jpg)

###找到目标APP

我们拿[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)举例。界面如下图所示：

![realsandbox2.jpg](http://7xkkk9.com1.z0.glb.clouddn.com/realsandbox2.jpg)

###浏览沙盒目录结构

####双击iOSStrongDemo

![realsandbox3.jpg](http://7xkkk9.com1.z0.glb.clouddn.com/realsandbox3.jpg)

####点击Show Container

设置图标单击之后，如图所示：

![realsandbox4.jpg](http://7xkkk9.com1.z0.glb.clouddn.com/realsandbox4.jpg)

这样我们只能看到沙盒的目录结构。看不到实体文件。那么，我们下一步就是找到这个沙盒。

###Download Container保存到桌面

如上图所示，有一个Download Container，选择之后，保存到桌面。

![realsandbox5.jpg](http://7xkkk9.com1.z0.glb.clouddn.com/realsandbox5.jpg)

###右键显示包内容

![realsandbox6.jpg](http://7xkkk9.com1.z0.glb.clouddn.com/realsandbox6.jpg)

###查看沙盒目录

AppDataInfo.plist是APP的配置文件：

![realsandbox7.jpg](http://7xkkk9.com1.z0.glb.clouddn.com/realsandbox7.jpg)

###打开AppData

![realsandbox8.jpg](http://7xkkk9.com1.z0.glb.clouddn.com/realsandbox8.jpg)

这就是APP的沙盒，在这里我们可以对文件进行增删查改操作。更多内容以后继续慢慢分享。