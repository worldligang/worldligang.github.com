---
layout: post
title: 两个非常不错的自定义UIAlertView第三方类库
date: 2015-10-30 10:08:00 +0800
comments: true
categories: iOS开发
keywords: UIAlertView, LMAlertView,CustomIOSAlertView , iOS开发, 刚刚在线
description: UIAlertView大家都不陌生了。UIAlertView不可以添加控件，比如说：UILabel、UITextView等等，所以遇到这个需求，一般都要自定义UIAlertView来解决。

---

UIAlertView大家都不陌生了。UIAlertView不可以添加控件，比如说：UILabel、UITextView等等，所以遇到这个需求，一般都要自定义UIAlertView来解决。

我再github上找到了两个非常错的自定义UIAlertView第三方类库，分别是：LMAlertView和CustomIOSAlertView。

##LMAlertView使用简介

[LMAlertView](https://github.com/lmcd/LMAlertView)在github上有610个star。说明这个第三方的UIAlertView还不错，我大概看了一下用法也比较简单。

来看看两个个案例效果：

![1](https://camo.githubusercontent.com/eb9242282b703a847c5ca45ef47462ec507ba573/687474703a2f2f6c6d63642e6d652f4c4d416c657274566965772d637573746f6d4032782e706e67)
<!--more-->
![2](https://camo.githubusercontent.com/b1c95e3be84a8d277e1c542ca768b3bdebe043a6/687474703a2f2f6c6d63642e6d652f4c4d416c657274566965772d7265766965774032782e706e673f33)

###用CocoaPods安装LMAlertView

	pod 'LMAlertView'
	
###使用案例	
		
	LMAlertView *alertView = [[LMAlertView alloc] initWithTitle:@"iOSDevTip"
                                            message:@"关注一下"
                                           delegate:nil
                                  cancelButtonTitle:@"ok"
                                  otherButtonTitles:nil];

	// Add your subviews here to customise
	UIView *contentView = alertView.contentView;
	
	[alertView show];	
	
LMAlertView还保持着UIAlertView的创建方法，作者还比较用心。
	
##CustomIOSAlertView使用简介

[CustomIOSAlertView](https://github.com/wimagguc/ios-custom-alertview)比LMAlertView的star还多一些，有861个。同样我们先看一下效果：

![3](https://github.com/wimagguc/ios-custom-alertview/raw/master/Docs/screen.png)

###用CocoaPods安装CustomIOSAlertView
	
	pod 'CustomIOSAlertView', '~> 0.9.3'

###使用案例

	CustomIOSAlertView *alertView = [[CustomIOSAlertView alloc] init];	UIView *customView ..;

	[alertView setContainerView:customView];
 	[alertView show];

CustomIOSAlertView可以设置代理，还可以通过block来实现按钮的点击事件。功能相对比较齐全。

喜欢的童鞋可以star一下，以备不时之需。如果你觉得不错，记得分享给更多朋友哈，谢谢！