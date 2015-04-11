---
layout: post
title: "系统提供的dispatch方法"
date: 2015-03-13 17:41:11 +0800
comments: true
categories: iOS开发
keywords: gcd, dispatch, dispatch_group_async, block, dispatch_queue_t, dispatch_queue_create, dispatch_group_t, dispatch_group_notify, iOS开发, cocoapods, 个人博客, 刚刚在线
description: "系统提供的dispatch方法"

---

为了方便地使用GCD，苹果提供了一些方法方便我们将block放在主线程或后台线程执行，或者延后执行。使用的例子如下所示：

	 //  后台执行：
	 dispatch_async(dispatch_get_global_queue(0, 0), ^{
	      // something
	 });
	
	 // 主线程执行：
	 dispatch_async(dispatch_get_main_queue(), ^{
	      // something
	 });
	
	 // 一次性执行：
	 static dispatch_once_t onceToken;
	 dispatch_once(&onceToken, ^{
	     // code to be executed once
	 });
	
	 // 延迟2秒执行：
	 double delayInSeconds = 2.0;
	 dispatch_time_t popTime = dispatch_time(DISPATCH_TIME_NOW, delayInSeconds * NSEC_PER_SEC);
	 dispatch_after(popTime, dispatch_get_main_queue(), ^(void){
	     // code to be executed on the main queue after delay
	 });
	
dispatch_queue_t也可以自己定义，如要自定义queue，可以用dispatch_queue_create方法，示例如下：
	
	 // 自定义dispatch_queue_t
	 dispatch_queue_t urls_queue = dispatch_queue_create("blog.devtang.com", NULL);
	 dispatch_async(urls_queue, ^{  
	　 　// your code 
	 });
	 dispatch_release(urls_queue);
	
另外，GCD还有一些高级用法，例如让后台两个线程并行执行，然后等两个线程都结束后，再汇总执行结果。这个可以用dispatch_group_t、dispatch_group_async、dispatch_group_notify来实现，示例如下：

	 // 合并汇总结果
	 dispatch_group_t group = dispatch_group_create();
	 dispatch_group_async(group, dispatch_get_global_queue(0,0), ^{
	      // 并行执行的线程一
	 });
	 dispatch_group_async(group, dispatch_get_global_queue(0,0), ^{
	      // 并行执行的线程二
	 });
	 dispatch_group_notify(group, dispatch_get_global_queue(0,0), ^{
	      // 汇总结果
	 });
	 
文章来自唐巧《iOS开发进阶》

更多iOS开发相关技术请关注iOS开发微信公众号 iOS开发 ：

	iOSDevTip