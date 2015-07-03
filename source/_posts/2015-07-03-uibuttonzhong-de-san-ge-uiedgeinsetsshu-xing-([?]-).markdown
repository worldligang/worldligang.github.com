---
layout: post
title: "UIButton中的三个UIEdgeInsets属性（一）"
date: 2015-07-03 23:29:55 +0800
categories: iOS开发
keywords: UIEdgeInsets, UIEdgeInsets, contentEdgeInsets, titleEdgeInsets, imageEdgeInsets, 刚刚在线
description: UIEdgeInsets今天是周六，不知道正在看文章的你有没有在加班。没有加班的，就好好休息一下吧；加班的，我只能同情一下啦。今天我们来看看UIButton中的三个UIEdgeInsets属性。
 
---
随着时间的推移，随着我的坚持，公众号的订阅用户已经越来越多了。非常感谢大家的信任，我会再接再厉，努力为大家提供更多优秀的文章。

当初创建iOS开发（ **iOSDevTip** ），只是自己的兴趣。刚开始还不怎么会用微信公众号的后台，连发图文消息都不会。不知道怎么去编辑，当初的微信公众号后台也没有现在这么强大。我还以为就是个一对多的聊天工具呢。（😊）

慢慢的我学会了编辑文章，成为一个真正意义上的小编。每天负责为大家推送一些iOS相关的技术文章。每天到各大论坛、网站、大神的博客找一些好的文章。这个过程是漫长的，但是是值得的。

找文章的过程中，对自己来说，也是学习的好机会。如果没有这个公众号，我可能就看不到那么多的技术文章，也不能为大家推荐文章，自己也不会建立博客。

现在微信团队已经邀请我开通原创功能，以后，我就会多发一些原创内容。一个人的力量比较是有限的，订阅的用户如果有喜欢写技术文章的，欢迎头投稿给我，我的邮箱：**worldligang@163.com**

**相信我，你的付出也会有回报的。**

今天是周六，不知道正在看文章的你有没有在加班。没有加班的，就好好休息一下吧；加班的，我只能同情一下啦。今天我们来看看UIButton中的三个UIEdgeInsets属性。

##UIEdgeInsets是什么

UIEdgeInsets是什么？我们点进去看一下：

	typedef struct UIEdgeInsets {
	    CGFloat top, left, bottom, right;  // specify amount to inset (positive) for each of the edges. values can be negative to 'outset'
	} UIEdgeInsets;

UIEdgeInsets是个结构体类型。里面有四个参数，分别是：top, left, bottom, right。这四个参数表示距离上边界、左边界、下边界、右边界的距离。

##哪三个UIEdgeInsets属性

不知道大家发现没有，UIButton里面有三个UIEdgeInsets属性，分别是：

	@property(nonatomic)          UIEdgeInsets contentEdgeInsets UI_APPEARANCE_SELECTOR; // default is UIEdgeInsetsZero
	
	@property(nonatomic)          UIEdgeInsets titleEdgeInsets;                // default is UIEdgeInsetsZero

	@property(nonatomic)          UIEdgeInsets imageEdgeInsets;                // default is UIEdgeInsetsZero

contentEdgeInsets后面有个UI_APPEARANCE_SELECTOR是什么意思呢？

	提示：UI_APPEARANCE_SELECTOR标记的属性都支持通过外观代理来定制。

举例，设置UIButton的contentEdgeInsets属性，可以直接调用：

    [[UIButton appearance] setTitleEdgeInsets:UIEdgeInsetsMake(0, 0, 0, 0)];

关于UIButton三个UIEdgeInsets属性的作用，下一篇文章介绍。