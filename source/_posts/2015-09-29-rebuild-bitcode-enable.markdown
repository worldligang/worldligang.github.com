---
layout: post
title: 报错：You must rebuild it with bitcode enabled (Xcode setting ENABLE_BITCODE)（解决方案）
date: 2015-09-29 15:08:00 +0800
comments: true
categories: iOS开发
keywords: ENABLE_BITCODE, bitcode, enabled, iOS开发, 刚刚在线
description: 今天在家里闲来无事，我就用Xcode7.0建一个项目。然后，引入一个第三方库，竟然运行不起来。报错如下：You must rebuild it with bitcode enabled (Xcode setting ENABLE_BITCODE)


---


马上就要十一放假啦！大家是不是都很期待这一天到来，哈哈。相信有不少人跟我一样，早就请三天假回家了吧。希望大家十一都玩的开心噢！

今天在家里闲来无事，我就用Xcode7.0建一个项目。然后，引入一个第三方库，竟然运行不起来。报错如下：

![1](http://images.90159.com/09/error1.png)

我把错误提示复制出来，方便大家搜索：

	ld: 'XXX' does not contain bitcode. You must rebuild it with bitcode enabled (Xcode setting ENABLE_BITCODE), obtain an updated library from the vendor, or disable bitcode for this target. for architecture arm64
	clang: error: linker command failed with exit code 1 (use -v to see invocation)
	
Xcode7.0默认是开启Enable Bitcode的，如果你导入的一些第三方库在编译的时候没有Enable Bitcode 的话，就会导致报错。

解决方法有两种：

方法一：使用Enable Bitcode的第三方库

方法二：将工程中的Enable Bitcode设置为NO

![1](http://images.90159.com/09/error2.png)

如果你有更好的解决方法，记得告诉我噢！


	