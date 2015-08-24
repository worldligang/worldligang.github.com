---
layout: post
title: "iOS开发小技巧：刷新UITableView"
date: 2015-08-18 15:25:58 +0800
comments: true
categories: iOS开发
keywords: UITableView, 局部, 局部刷新, iOS开发, 刚刚在线
description: UITableView对于iOS开发者来说一定不会陌生，很有可能你的APP很多界面都用到它。关于UITableView的文章，想必已经不计其数，没事可以多看看。特别是UITableView优化的文章，非常值得仔细琢磨一番。

---

>我创建了iOS开发博主联盟群：**364679270**，有博客的朋友欢迎加入。一起打造一个优质的iOS技术分享社群。


UITableView对于iOS开发者来说一定不会陌生，很有可能你的APP很多界面都用到它。关于UITableView的文章，想必已经不计其数，没事可以多看看。特别是UITableView优化的文章，非常值得仔细琢磨一番。

今天我们来看看如何刷新UITableView的，一般情况下，刷新UITableView，我们会直接调用reloadData方法。

<!--more-->

##刷新UITableView

	[self.tableView reloadData];

reloadData是刷新整个UITableView，有时候，我们可能需要局部刷新。比如：只刷新一个cell、只刷新一个section等等。这个时候在调用reloadData方法，虽然用户看不出来，但是有些浪费资源。



##刷新局部cell

     NSIndexPath *indexPath = [NSIndexPath indexPathForRow:0 inSection:0];
     [self.tableView reloadRowsAtIndexPaths:[NSArray arrayWithObjects:indexPath,nil] withRowAnimation:UITableViewRowAnimationFade];
     
这样就可以很方便的刷新第一个section的第一个cell。虽然看起来代码多了，但是确实比较节省资源。尽量少的刷新，也是UITableView的一种优化。    
     
##局部刷新section

	NSIndexSet *indexSet = [[NSIndexSet alloc] initWithIndex:0];
	[self.tableView reloadSections:indexSet withRowAnimation:UITableViewRowAnimationFade];

上面这段代码是刷新第0个section。

##刷新动画

刷新UITableView还有几个动画：

	typedef NS_ENUM(NSInteger, UITableViewRowAnimation) {
	    UITableViewRowAnimationFade,   //淡入淡出
	    UITableViewRowAnimationRight,  //从右滑入         // slide in from right (or out to right)
	    UITableViewRowAnimationLeft,   //从左滑入
	    UITableViewRowAnimationTop,		//从上滑入
	    UITableViewRowAnimationBottom,	//从下滑入
	    UITableViewRowAnimationNone,            // available in iOS 3.0
	    UITableViewRowAnimationMiddle,          // available in iOS 3.2.  attempts to keep cell centered in the space it will/did occupy
	    UITableViewRowAnimationAutomatic = 100  // available in iOS 5.0.  chooses an appropriate animation style for you
	};
	
	