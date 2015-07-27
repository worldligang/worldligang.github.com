---
layout: post
title: "五种创建UIImage的类方法"
date: 2015-07-28 01:08:30 +0800
comments: true
categories: iOS开发
keywords: UIImage, imageNamed, imageNamed, iOS开发, 刚刚在线
description: UIImage有四个类方法，用来创建UIImage的。下面介绍一下每个类方法的作用和创建实例。

---

###iOS开发已开通评论功能

皇天不负有心人，iOS开发的评论功能已经被邀请开通啦。这几天一直想着，微信团队大概在月底的时候会邀请我开通评论功能。果然不出我所料。这是我收到的消息：

![weixincomment.png](http://7xkkk9.com1.z0.glb.clouddn.com/weixincomment.png)

至此，iOS开发的原创、赞赏、评论三个功能都齐啦，这也是对我辛苦努力的一种肯定。前方的路依然很漫长，我们一起努力，让iOS开发变得更好。

既然评论功能已经开通，大家也别闲着啦！今天的文章大家一起尽情的吐槽吧！

`UIImage`有四个类方法，用来创建`UIImage`的。下面介绍一下每个类方法的作用和创建实例。

###1.使用类方法`imageNamed:`创建

	+ (UIImage *)imageNamed:(NSString *)name;      // load from main bundle      

`name`是照片名称。创建UIImage对象，代码如下：   
    
    UIImage *image = [UIImage imageNamed:@"ganggang"];   

使用`imageNamed:`初始化的时候，会先检查缓存中是否存在`ganggang`的照片，如不存在，图片首先会被缓存起来，然后才返回要加载的图片对象；如果存在，直接返回要加载的照片对象。

###2.使用类方法`imageWithContentsOfFile:`创建
	
	+ (UIImage *)imageWithContentsOfFile:(NSString *)path;

`path`是需要加载照片的路径，如何获取照片的路径请点击[iOS开发之获取沙盒路径](http://www.superqq.com/blog/2015/07/22/ioskai-fa-zhi-huo-qu-sha-he-lu-jing/)。实现代码如下：

	UIImage *image = [UIImage imageWithContentsOfFile:path];   

使用`imageWithContentsOfFile:`创建`UIImage`的时候，是直接从磁盘上加载。当收到内存警告时，`UIImage`对象会被释放，下一次绘图的时候，需要重新加载。

###3.使用类方法`imageWithData:`创建

	+ (UIImage *)imageWithData:(NSData *)data;

`data`是照片数据，一般是请求返回的，然后通过`imageWithData:`创建`UIImage`。实现代码如下所示：    
    
    NSData *data = [NSData dataWithContentsOfURL:[NSURL URLWithString:@"http://www.superqq.com/images/getqrcode.jpg"]];
    UIImage *image =[UIImage imageWithData:data]; 

###4.使用类方法`imageWithCGImage:`创建

	+ (UIImage *)imageWithCGImage:(CGImageRef)cgImage;

`cgImage`是定义在QuartzCore框架中的一个结构体指针。这个结构用来创建像素位图，可以通过操作存储的像素位来编辑图片。 
    
    UIImage *image = [UIImage imageWithCGImage:cgImage];   
 

###5.使用类方法`imageWithCIImage:`创建
      
	+ (UIImage *)imageWithCIImage:(CIImage *)ciImage NS_AVAILABLE_IOS(5_0);

`CIImage`是`CoreImage`框架中最基本代表图像的对象，在`CIImage`被`CIContext`渲染出来之前，他是依赖于滤镜链的，滤镜是不会更改`CIImage`中的图像数据。
 
     UIImage *image = [UIImage imageWithCGImage:ciImage];   

以上四种方法都是`UIImage`的类方法，使用`UIImage`的初始化方法也是可以创建`UIImage`对象的大家可以进`UIImage`详细了解一下。   
