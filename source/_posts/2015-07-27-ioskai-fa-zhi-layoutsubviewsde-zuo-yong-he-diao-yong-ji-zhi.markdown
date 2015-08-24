---
layout: post
title: "iOS开发之layoutSubviews的作用和调用机制"
date: 2015-07-27 13:57:27 +0800
comments: true
categories: iOS开发
keywords: layoutSubviews, iOS layoutSubviews, setNeedsLayout, iOS开发, 刚刚在线
description: 在UIView里面有一个方法`layoutSubviews`，这个方法具体作用是什么呢？改变一个`UIView`大小的时候也会触发父`UIView`上的`layoutSubviews`事件。

---
在UIView里面有一个方法`layoutSubviews`，这个方法具体作用是什么呢？

		
	- (void)layoutSubviews;    // override point. called by layoutIfNeeded automatically. As of iOS 6.0, when constraints-based layout is used the base implementation applies the constraints-based layout, otherwise it does nothing.
	
##官方文档解释
	
我们先来看看苹果官方文档的解释：
	
<!--more-->

	The default implementation of this method does nothing on iOS 5.1 and earlier. Otherwise, the default implementation uses any constraints you have set to determine the size and position of any subviews.
	
	Subclasses can override this method as needed to perform more precise layout of their subviews. You should override this method only if the autoresizing and constraint-based behaviors of the subviews do not offer the behavior you want. You can use your implementation to set the frame rectangles of your subviews directly.
	
	You should not call this method directly. If you want to force a layout update, call the setNeedsLayout method instead to do so prior to the next drawing update. If you want to update the layout of your views immediately, call the layoutIfNeeded method.

最后一段说，不要直接调用此方法。如果你想强制更新布局，你可以调用`setNeedsLayout`方法；如果你想立即数显你的`views`，你需要调用`layoutIfNeeded`方法。
 
##layoutSubviews作用

`layoutSubviews`是对`subviews`重新布局。比如，我们想更新子视图的位置的时候，可以通过调用`layoutSubviews`方法，既可以实现对子视图重新布局。

`layoutSubviews`默认是不做任何事情的，用到的时候，需要在自雷进行重写。

##layoutSubviews以下情况会被调用

苹果官方文档已经强调，不能直接调用`layoutSubviews`对子视图进行重新布局。那么，`layoutSubviews`什么情况下会被调用呢？通过百度搜索，发现以下几种情况`layoutSubviews`会被调用。

1. 直接调用`setLayoutSubviews`。（这个在上面苹果官方文档里有说明）
2. `addSubview`的时候。
3. 当`view`的`frame`发生改变的时候。
4. 滑动`UIScrollView`的时候。
5. 旋转`Screen`会触发父`UIView`上的`layoutSubviews`事件。
6. 改变一个`UIView`大小的时候也会触发父`UIView`上的`layoutSubviews`事件。
 
我简单测试了一下，上面基本都会被调用。 注意：

	当view的fram的值为0的时候，`addSubview`也不会调用`layoutSubviews`的。

`layoutSubviews`方法在对自雷视图进行布局的时候非常方便。可以自己动手，深入理解`layoutSubviews`的调用机制。
