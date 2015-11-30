---
layout: post
title: iOS提交审核：您的 App 正在使用广告标识符 (IDFA)
date: 2015-11-13 10:34:12 +0800
comments: true
categories: iOS开发
keywords: 审核, IDFA,广告标识符 , iOS开发, 刚刚在线
description: 从14年2月初开始，Apple开始拒绝采集IDFA(identifier for advertising)而未集成任何广告服务的应用进入AppStore。怎么看自己的项目是否采集了IDFA呢？方法很简单，大家去看framework，看看有没有一个framework叫AdSupport.framework。

---


提交审核也是iOS开发者必备技能之一。关于提交审核的教程数不胜数，不会的童鞋赶紧尝试一下吧！

这篇文章我们来解决一个问题，关于IDFA的：

	您的 App 正在使用广告标识符 (IDFA)。您必须先提供关于 IDFA 的使用信息或将其从 App 中移除，然后再上传您的二进制文件。

从14年2月初开始，Apple开始拒绝采集IDFA(identifier for advertising)而未集成任何广告服务的应用进入AppStore。怎么看自己的项目是否采集了IDFA呢？方法很简单，大家去看framework，看看有没有一个framework叫AdSupport.framework。

如果有，那就惨了，上两张图看看：

![idfa1](http://images.90159.com/11/idfa1.png)
<!--more-->

如果你选择了“否”：

![idfa2](http://images.90159.com/11/idfa2.png)


##方案一：重新下载xcode

1. 如果你的xcode不是最新版本的，建议下载最新版本的。重新打包后，再次提交审核。
2. 如果你的xcode不是正规渠道下载的，建议从苹果官方下载。

有朋友尝试过方案一，问题得到解决。如果解决不了，还有方案二。


##方案二：检测哪里用到IDFA

首先cd到你的工程目录下，然后执行命令：

	grep -r advertisingIdentifier .

果然找到了罪魁祸首，原来是SinaWeiboSDK/libWeiboSDK.a

![idfa3](http://images.90159.com/11/idfa3.png)

ios9之后新浪微博分享可使用的前提是加入ADSupport.framework,打包提交后一直报您的 App 正在使用广告标识符 (IDFA)。您必须先提供关于 IDFA 的使用信息或将其从app中移除，再上传二进制文件。

找到了源头，那么接下来怎么做就简单了。

1. 承认使用了IDFA，然后选择相应的选项。
2. 移除SinaWeiboSDK/libWeiboSDK.a，分享还是可以成功的。