---
layout: post
title: "iOS开发：UITableView加载多张照片导致内存上涨的问题"
date: 2014-11-06 10:14:54 +0800
comments: true
categories: iOS开发
keywords: iOS, UITableView, 内存上涨, iOSDevTip iOS开发, gcd, cocoapods, 队列, 个人博客, 刚刚在线
description: "iOS开发：UITableView加载多张照片导致内存上涨的问题"  

---
<a href="http://www.superqq.com/blog/2014/11/06/ioskai-fa-:uitableviewjia-zai-duo-zhang-zhao-pian-dao-zhi-nei-cun-shang-zhang-de-wen-ti/">iOS开发：UITableView加载多张照片导致内存上涨的问题</a>


最近在写一个文件管理的页面，里面功能挺多的，有缩略图下载（socket），有文件下载（http）。


说说缩略图下载这一块，我的UITableView的一个cell要加载四张缩略图，iPhone5s的屏幕能加载8行。大概UI上是这样布局的。（另外缩略图需要一张张下载）


刚开始进到文件管理界面，内存占用20几M，随着缩略图不断下载下来内存在上涨，我不断的往上拉，内存一值在涨。达到一定值app出现闪退。大概是300多M的时候闪退。什么原因造成的呢？


先看下图：

<img src="http://www.superqq.com/images/neicun1.jpg" >


我是这样写的：建立一个对象，这个对象有一个属性是UIImage。对象保存在数组里面。大概有800多张照片。


当缩略图没有下载下来的时候，我加载的是一张默认图片。当缩略图下载下来的时候，我就把下载下来的缩略图赋给相应对象的UIImage属性。


cell是重用的，这样看应该是没有什么问题的。但是内存就是不断上涨，而且是不动的时候，内存不涨，上下滑动UITableView，内存不断上涨。


我一直在思考cell是重用的，内存怎么会不断上涨呢？问题不是cell引起的，而是上面我说的那个对象的UIImage属性引起的。


思考一下，缩略图不断下载下来， 下载下来的缩略图都赋给对象的UIImage属性，而对象又在数组里面，可想而知，800多张缩略图不断下载内存肯定上涨。


于是，我去掉对象的UIImage属性，把下载下来的照片通过EGOCache缓存起来，加载cell上的UIImage的时候，直接从缓存里面取出来就可以了。


代码如下：


	[[EGOCache globalCache] setImage:image forKey:[NSString stringWithFormat:@"EGOImageLoader-%lu", (unsigned long)[[fmObject.path description] hash]] withTimeoutInterval:CacheingTime];


再次运行起来，看一下内存这一块：

<img src="http://www.superqq.com/images/neicun2.png" >



果然，问题解决了。看来以后给对象加UIImage属性需谨慎啊！


大家有问题可以加入iOS学习交流群：

	303868520


微信公众账号：iOS开发
	
	iOSDevTip


