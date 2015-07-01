---
layout: post
title: "EGOCache缓存框架详细讲解"
date: 2015-07-01 00:10:18 +0800
comments: true
categories: iOS开发
keywords: EGOCache, EGOCache缓存, EGOCache缓存框架, EGOCache使用, 刚刚在线
description: EGOCache是一个轻量级的缓存框架。用法简单方便，在现在的项目中，我就用到EGOCache来缓存下载过的照片和字符串。有人可能会问到，缓存照片还需要用EGOCache吗？AFNetworking和SDWebImage不是已经有这些功能了吗？

---

**EGOCache**是一个轻量级的缓存框架。用法简单方便，在现在的项目中，我就用到EGOCache来缓存下载过的照片和字符串。

有人可能会问到，缓存照片还需要用EGOCache吗？[AFNetworking](http://www.superqq.com/blog/2014/11/07/ioswang-luo-bian-cheng-zhi-afnetworkingshi-yong/)和[SDWebImage](http://www.superqq.com/blog/2015/06/09/sdwebimagejia-zai-tu-pian-huan-cun-tu-pian/)不是已经有这些功能了吗？

是的，不过AFNetworking和SDWebImage是http。我的项目用的是socket，所以我选择EGOCache来做缓存。用下来觉得EGOCache还是挺强大的。

##EGOCache简介

	EGOCache is a simple, thread-safe key value cache store. It has native support for NSString, UI/NSImage, and NSData, but can store anything that implements <NSCoding>. All cached items expire after the timeout, which by default, is one day.
	
翻译过来就是：EGOCache一个简单、线程安全的基于 key-value 的缓存框架，原生支持NSString、UI/NSImage、和NSData，也支持储存任何实现<NSCoding>协议的类，可以设定缓存过期时间，默认是1天。

EGOCache只有一个类，EGOCache.h和EGOCache.m两个文件。用法也比较容易掌握，仔细研究一下EGOCache.h的方法，很快就可以上手。

EGOCache只提供了磁盘缓存，没有提供内存缓存。同时，也提供了清理缓存的方法：

	- (void)clearCache;

EGOCache还提供了判断缓存是否存在的方法：

	- (BOOL)hasCacheForKey:(NSString* __nonnull)key;

##通过Cocoapods直接加入项目

直接在你的项目的Podfile加入下面一行：
	
	pod 'EGOCache'
	 
然后执行：

	$ pod update
	


##EGOCache用法

###用EGOCache缓存NSString

存储：
	
	NSString *saveString = @"把我保存起来吧";
    [[EGOCache globalCache] setString:saveString forKey:[NSString stringWithFormat:@"EGOImageLoader-%lu", (unsigned long)[saveString hash]] withTimeoutInterval:24*60*60];
    
读取：

	NSString *getSaveString = [[EGOCache globalCache] stringForKey:[NSString stringWithFormat:@"EGOImageLoader-%lu", (unsigned long)[@"SaveString" hash]]];

是不是感觉跟NSDictionary很相似，确实，前面我们说了EGOCache是基于key-value 的缓存框架。

###用EGOCache缓存UIImage
 
存储：

	 UIImage *saveImage = [UIImage imageNamed:@"iOSDevTip"];
    [[EGOCache globalCache] setImage:saveImage forKey:[NSString stringWithFormat:@"EGOImageLoader-%lu", (unsigned long)[@"SaveImage" hash]] withTimeoutInterval:24*60*60];
    
读取：

	UIImage *getSaveImage = [[EGOCache globalCache] imageForKey:[NSString stringWithFormat:@"EGOImageLoader-%lu", (unsigned long)[@"SaveImage" hash]]];

###用EGOCache缓存NSData

存储：

	NSData *saveData = [NSData data];
    [[EGOCache globalCache] setData:saveData forKey:[NSString stringWithFormat:@"EGOImageLoader-%lu", (unsigned long)[@"SaveData" hash]] withTimeoutInterval:24*60*60];

读取：

    UIImage *getSaveData = [[EGOCache globalCache] dataForKey:[NSString stringWithFormat:@"EGOImageLoader-%lu", (unsigned long)[@"SaveData" hash]]];

##EGOCache源码下载

[EGOCache](https://github.com/enormego/EGOCache)