---
layout: post
title:  为自己的应用配备上 3D Touch 功能 
date: 2015-10-31 10:35:24 +0800
comments: true
categories: 推荐
keywords: 3D Touch, 应用,iPhone 6s Plus , iOS开发, 刚刚在线
description: 随着 iPhone 6s 以及 iPhone 6s Plus 的发布，开发者们现在就可以为自己的应用配备上 3D Touch 功能了，从而给界面交互方式开启一个新的维度。


---



随着 iPhone 6s 以及 iPhone 6s Plus 的发布，开发者们现在就可以为自己的应用配备上 3D Touch 功能了，从而给界面交互方式开启一个新的维度。

正如苹果所言，开发者可以通过非常简单的 API 来使用 3D Touch ，从根本上来说，也就是 UITouch 的一个简单的新属性。

	override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
	     guard let touch = touches.first else { return }
	     if traitCollection.forceTouchCapability == .Available {
	        println("Touch pressure is \(touch.force), maximum possible force is \(touch.maximumPossibleForce)")
	     }
	}

这个新的 API 可以让应用发挥出巨大的潜力，比如说游戏中的额外控制选项、绘图应用中的细粒度(fine-grained)控制，甚至是用来替代我们在 iOS 设备中使用过的长按操作(tap-and-hold)的极佳选择。

除了 UITouch 中新增的 API 外，苹果还为应用提供了两个用来增加3D Touch 功能的类集：UIPreviewAction 和 UIApplicationShortcutItem。

UIPreviewAction允许开发者在用户使用 3D Touch 功能触控一个 UI 元素的时候，快速地在一个新的预览窗口中显示某些内容。这种快速浏览应用特定内容的方式真的非常棒，比如说我们可以快速预览邮件信息、照片，甚至是网页内容，而无需弹出一个完整的视图控制器。

UIApplicationShortcutItem对象能够让 iOS 主屏幕激活一项令人惊叹的新特性。当用户使用 3D Touch 按下某个应用的图标时，一个选项列表就会被弹出，允许用户快速跳转至应用的特定部分，或者执行某项应用内的功能。

![touch](http://images.90159.com/10/touch.jpg)

总而言之，3D Touch 的引入给 iOS 设备解锁了一个全新的交互方式，并且将会给接下来的 iOS 应用带来新一代的创新。关于3D Touch 的实例代码和相关信息可以在苹果开发者网站的3D Touch网页上找到，祝你好运！

作者：Tim Oliver
@TimOliverAU — iComics创始人
