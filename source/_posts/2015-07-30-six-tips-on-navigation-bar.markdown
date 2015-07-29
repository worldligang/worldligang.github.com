---
layout: post
title: "关于导航栏的六个小技巧"
date: 2015-07-30 00:13:43 +0800
comments: true
categories: iOS开发
keywords: 导航栏, 导航, 开发技巧, iOS开发, 刚刚在线
description: UINavigationBar和UINavigationItem是iOS开发中常用的控件。今天给大家介绍一下六个关于导航栏的小技巧。

---

`UINavigationBar`和`UINavigationItem`是iOS开发中常用的控件。今天给大家介绍一下六个关于导航栏的小技巧。


##1.设置导航栏标题

	self.title = @"iOS开发：iOSDevTip";

##2.设置导航栏样式

设置方法：
	       
	[self.navigationController.navigationBar setBarStyle:UIBarStyleBlack];

`UIBarStyle`的样式：	
	
	typedef NS_ENUM(NSInteger, UIBarStyle) {
	    UIBarStyleDefault          = 0,
	    UIBarStyleBlack            = 1,
	    
	    UIBarStyleBlackOpaque      = 1, // Deprecated. Use UIBarStyleBlack
	    UIBarStyleBlackTranslucent = 2, // Deprecated. Use UIBarStyleBlack and set the translucent property to YES
	};

`UIBarStyleDefault`是默认样式，`UIBarStyleBlack`是黑色不透明。`UIBarStyleBlackOpaque`和`UIBarStyleBlackTranslucent`这两个已经废弃了。

如果想设置导航栏透明，可以加上下面这句代码：

	self.navigationController.navigationBar.translucent = YES;

##3.修改返回按钮title

    self.navigationItem.title = @"test";

##4.隐藏返回按钮title

比较笨的方法是：

    self.navigationItem.title = @"";

还可以这样设置：

	[[UIBarButtonItem appearance] setBackButtonTitlePositionAdjustment:UIOffsetMake(0, -60) forBarMetrics:UIBarMetricsDefault];

##5.设置leftBarButtonItem

	self.navigationItem.leftBarButtonItem = [[UIBarButtonItem alloc] initWithTitle:@"back" style:UIBarButtonItemStylePlain target:self action:@selector(back:)];

	- (void)back:(id)sender
	{
	    [self.navigationController popViewControllerAnimated:YES];
	}
	
##6.左滑返回手势失效了怎么办

如果按上一步设置`leftBarButtonItem`之后，左滑返回手势就会失效。设置一下`UIGestureRecognizerDelegate`代理即可：

	self.navigationController.interactivePopGestureRecognizer.delegate = self;

以上是六个关于导航栏的小技巧。更多iOS开发相关的技术，请关注我的微信公众号iOS开发：**iOSDevTip**
