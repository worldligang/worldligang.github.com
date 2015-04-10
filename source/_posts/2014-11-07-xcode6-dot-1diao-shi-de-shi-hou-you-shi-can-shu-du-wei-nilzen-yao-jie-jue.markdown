---
layout: post
title: "xcode6.1调试的时候有时参数都为nil怎么解决?"
date: 2014-11-07 09:18:04 +0800
comments: true
categories: iOS开发
keywords: iOS, xcode6.1, 调试,xcode6.1调试的时候有时参数都为nil怎么解决?, nil, iOSDevTip iOS开发, gcd, cocoapods, 队列, 个人博客, 刚刚在线
description: "xcode6.1调试的时候有时参数都为nil怎么解决?" 

---
<a href="http://www.superqq.com/blog/2014/11/07/xcode6-dot-1diao-shi-de-shi-hou-you-shi-can-shu-du-wei-nilzen-yao-jie-jue/">xcode6.1调试的时候有时参数都为nil怎么解决?</a>

昨天我在写代码的时候，发现xcode6.1调试的时候有时参数都为nil？曾经有朋友遇到过这个问题，我一直没当回事，昨天我也碰到了：


看下图：

<img src="http://mmbiz.qpic.cn/mmbiz/8RTSPr4mlylSosnUuLhV6wTjsFzyRRxoxIUFMqUwfUBic9qDUm8IY9gJQibL0oNeGUMCKN1oVJBwnv6l5OAezkicg/640" >

刚创建的vc 和 nav 都为nil，这给断点调试增加了很多麻烦，难道是xcode的bug吗？


群里问问大牛，不得姐，网上有查，最终找到解决方法：


在Project的Build Settings中把Optimization Level 设置成 None 即可。

<img src="http://mmbiz.qpic.cn/mmbiz/8RTSPr4mlylSosnUuLhV6wTjsFzyRRxo44fXntXf13uoSlnLOsdTkbhHFsHXeDljz69g3JR4sEJnr5SRSLO8icQ/640" >

在运行看一下结果：

<img src="http://mmbiz.qpic.cn/mmbiz/8RTSPr4mlylSosnUuLhV6wTjsFzyRRxoHE3BZhvRDgzxlCG8ibiceSGl81ykIM58AejX3nYW5YO5aSTwTsick82Sw/640" >

果然，有值了。小问题的解决方法，分享给大家。



喜欢我们的内容，可以点击右上角「分享到朋友圈」



以后大家有什么问题，可以登录<a href="http://www.iswifting.com">swift社区</a> 里面设置有问答区，会有大牛帮大家解答问题的。


微信公众账号：iOS开发

	iOSDevTip

QQ群：iOS学习交流群

	303868520



