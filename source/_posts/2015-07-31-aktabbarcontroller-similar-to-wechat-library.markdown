---
layout: post
title: "AKTabBarController：类似微信的第三方库"
date: 2015-07-31 00:28:53 +0800
comments: true
categories: iOS开发
keywords: 导航栏, 微信导航, 开发技巧, iOS开发, 刚刚在线
description: 其实，这位朋友的问题不是关于自定义导航栏，我觉得他可能是想问微信底部的`TabBar`和导航结合使用是如何实现的。


---

自从有了评论之后，就可以很方便的和大家交流。以前都是我一个人默默的发文章，看不出大家对文章的看法。评论开通之后，就可以很快速的获取大家对文章的看法。

有人会指出文章里有错误的地方，真的很感激他们，非常的认真；有人会对文章说出自己的见解，加深自己的印象，同时，也能帮助到其他的朋友；有人也会在评论里，提出一些问题，但是鉴于评论的字数限制，只能简单的回答一下。

在昨天的文章评论里面，有一个朋友提出了一个问题，关于微信导航栏的。

##问题分析和解答


**问：**

	自定义导航栏怎么写？就像微信那样，tabBar的四个界面切换导航栏都不变化

**答：**	

	github上搜索AKTabBarController
	

其实，这位朋友的问题不是关于自定义导航栏，我觉得他可能是想问微信底部的`TabBar`和导航结合使用是如何实现的。

记得之前在github看到过有一个类似微信的框架（算不上框架，我们就这么简单的叫一下）。就是[AKTabBarController](https://github.com/alikaragoz/AKTabBarController)。

`AKTabBarController`是一个非常不错的第三方。在github上有663个`star`，使用方法也不是很复杂，下面我们简单的看一下。

##如何导入到项目中

`AKTabBarController`是支持[CocoaPods](http://www.superqq.com/blog/2014/10/16/cocoapodsan-zhuang-he-shi-yong-jiao-cheng/)导入的。想理解CocoaPods的请点击：[CocoaPods安装和使用教程](http://www.superqq.com/blog/2014/10/16/cocoapodsan-zhuang-he-shi-yong-jiao-cheng/)。在你的Podfile文件中加入：

	platform :ios

	pod 'AKTabBarController'
	
添加成功之后，在终端执行：

	pod install 

##如何使用AKTabBarController

安装成功之后，在你想用的地方导入头文件：

	#import "AKTabBarController.h"

然后，创建`tabBar`

	// Create and initialize the height of the tab bar to 50px.
	_tabBarController = [[AKTabBarController alloc] initWithTabBarHeight:50];
	
	// Create and initialize with specific height and position atop the view controller
	_tabBarController = [[AKTabBarController alloc] initWithTabBarHeight:50 position:AKTabBarPositionTop];
	
	// Adding the view controllers to manage.
	[_tabBarController setViewControllers:@[[[FirstViewController alloc] init], [[SecondViewController alloc] init], [[ThirdViewController alloc] init], [[FourthViewController alloc] init]]]];  


关于更多AKTabBarController的用法，请访问：**https://github.com/alikaragoz/AKTabBarController**

##功能简介


    • 自适应竖屏和横屏模式，tab 上的图标会根据竖屏和横屏模式自动缩放大小以适应tab的高度。
    
    • 自定义 tab bar 的高度；
    
    • 当进入下个视图时，可以自动隐藏tab bar；
    
    • 可以只显示图片，而不显示文字；并且当 tab bar 的高度太小时，可以自动缩放图片以及隐藏文字；
    
    • 只需要一张图片来表示 tab 被选择和非选择模式。即tab 被选中时的高光是用CoreGraphics绘制的，不需要额外的图片。
    
    • 选择不同 tab 时的动画切换效果用的是淡入淡出（cross fade）的效果. 
    
    • 支持预渲染图像
    
    • 支持设置tabBar字体。

##实际效果
	
最终实现的效果如下所示：	

![1](https://github.com/alikaragoz/AKTabBarController/raw/master/Screenshots/iphone-portrait.png)

这里只是简单的讲解一下。大家闲的时候，如果有兴趣的话，可以自己动手来熟练关于`AKTabBarController`更多的功能。	