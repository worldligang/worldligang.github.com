---
layout: post
title: "单例模式的两种写法"
date: 2015-03-21 17:39:16 +0800
comments: true
categories: iOS开发
keywords: gcd, dispatch, 单例, 单例模式, dispatch_once, allocWithZone, sharedManager, iOS开发, cocoapods, 个人博客, 刚刚在线
description: "单例模式的两种写法"

---

单例模式是iOS开发中最常用的设计模式，iOS的单例模式有两种官方写法，如下：

###1，常用写法

	
	#import "LGManagerCenter.h"
	
	static LGManagerCenter *managerCenter;
	
	@implementation LGManagerCenter
	
	+(LGManagerCenter *)sharedManager{
	    if(!managerCenter)
	        managerCenter=[[self allocWithZone:NULL] init];
	    return  managerCenter;
	}
	
	@end
	
	
###2,用GCD创建单例类

	
	#import "LGManagerCenter.h"


	@implementation LGManagerCenter
	
	
	+(LGManagerCenter *)sharedManager{
	    static dispatch_once_t predicate;
	    static LGManagerCenter * managerCenter;
	    dispatch_once(&predicate, ^{
	        managerCenter=[[LGManagerCenter alloc] init];
	    });
	    return managerCenter;
	}
	
	@end
	
其中dispatch_once函数只执行一次。

更多iOS开发相关技术请关注iOS开发微信公众号 iOS开发 ：

	iOSDevTip