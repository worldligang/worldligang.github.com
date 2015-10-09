---
layout: post
title: "UIViewController的edgesForExtendedLayout属性"
date: 2015-10-09 16:36:41 +0800
comments: true
categories: iOS开发
keywords: edgesForExtendedLayout, UIViewController, UIRectEdgeNone, iOS开发, 刚刚在线
description: 是不是很讨厌！其实，在iOS 7中，UIViewController引入了一个新的属性：edgesForExtendedLayout。 这个属性的默认值是UIRectEdgeAll。当你的容器是UINavigationController的shih，默认的布局就是从状态栏的顶部开始的。这就是为什么你设置的控件都往上漂移了66ot的原因。

---

想必大家都遇到一种情况，明明y坐标设置的是0，但是总是被讨厌的导航栏给遮住。比如下面这个情况：
 
    UILabel *label = [[UILabel alloc] init];
    label.frame = CGRectMake(10, 0, SCREEN.width - 20, 88);
    label.backgroundColor = [UIColor redColor];
    label.text = @"关注公众号iOS开发：iOSDevTip";
    label.textColor = [UIColor whiteColor];
    label.textAlignment = NSTextAlignmentCenter;
    [self.view addSubview:label];
 
一起来看看效果：

![RectEdge2](http://images.90159.com/10/RectEdge2.png)

是不是很讨厌！其实，在iOS 7中，UIViewController引入了一个新的属性：edgesForExtendedLayout。 这个属性的默认值是UIRectEdgeAll。当你的容器是UINavigationController的shih，默认的布局就是从状态栏的顶部开始的。这就是为什么你设置的控件都往上漂移了66ot的原因。 
 
   	@property(nonatomic,assign) UIRectEdge edgesForExtendedLayout NS_AVAILABLE_IOS(7_0); // Defaults to UIRectEdgeAll

那么如何解决这个问题呢？有两种方法。

##方法一：改变edgesForExtendedLayout

	self.edgesForExtendedLayout = UIRectEdgeNone;

将edgesForExtendedLayout属性设置为UIRectEdgeNone，这样布局就是从导航栏下面开始了。设置之后，再来看看效果：

![RectEdge1](http://images.90159.com/10/RectEdge1.png)


##方法二：导航栏半透明属性设置为NO

	@property(nonatomic,assign,getter=isTranslucent) BOOL translucent NS_AVAILABLE_IOS(3_0) UI_APPEARANCE_SELECTOR; // Default is NO on iOS 6 and earlier. Always YES if barStyle is set to UIBarStyleBlackTranslucent

在iOS 6之前（包括iOS 6）translucent默认就是NO，在iOS 7就默认是YES了。

    self.navigationController.navigationBar.translucent = NO;
    
将导航栏的半透明属性关闭掉，布局也是从导航栏下面开始了。
