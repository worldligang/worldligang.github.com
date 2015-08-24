---
layout: post
title: "iOS开发之获取沙盒路径"
date: 2015-07-22 01:02:41 +0800
comments: true
categories: iOS开发
keywords: 沙盒机制, 沙盒路径, 沙盒, iOS开发, 刚刚在线
description: iOS开发之沙盒机制（SandBox)详细讲解了沙盒的一些机制。在开发中，我们需要对沙盒进行操作，所以我们需要获取到沙盒路径。

---

[iOS开发之沙盒机制（SandBox)](http://www.superqq.com/blog/2015/07/20/ioskai-fa-zhi-sha-he-ji-zhi-%28sandbox/)详细讲解了沙盒的一些机制。在开发中，我们需要对沙盒进行操作，所以我们需要获取到沙盒路径。

沙盒里的文件夹包括Documents、Library、tmp。这三个文件夹的作用请点击[这里](http://www.superqq.com/blog/2015/07/20/ioskai-fa-zhi-sha-he-ji-zhi-%28sandbox/)。接下来我们来讲解如何获取Documents、Library、tmp的路径。

###获取沙盒根目录

获取沙盒根目录，直接调用NSHomeDirectory()：

 	//获取沙盒根目录
    NSString *directory = NSHomeDirectory();
    NSLog(@"directory:%@", directory);

<!--more-->

控制台输出：    

	2015-07-22 00:40:16.185 iOSStrongDemo[1605:555658] directory:/var/mobile/Containers/Data/Application/F9418815-51A9-4A0A-A76C-6FD37C400928

这个是真机的路径，大家有时间的话可以看看模拟器的根目录路径。

###获取Documents路径

获取Documents路径如下：

	//获取Documents路径
    NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
    NSString *path = [paths objectAtIndex:0];
    NSLog(@"path:%@", path);
    
控制台输出：        
    
    2015-07-22 00:41:41.397 iOSStrongDemo[1613:556159] path:/var/mobile/Containers/Data/Application/A62B886B-A8F0-4215-B59D-1F505C3997BD/Documents
    
获取Documents文件夹目录,第一个参数是说明获取Doucments文件夹目录，第二个参数说明是在当前应用沙盒中获取。    

###获取Library路径
 	
 	//获取Library路径
    NSArray *paths = NSSearchPathForDirectoriesInDomains(NSLibraryDirectory, NSUserDomainMask, YES);
    NSString *path = [paths objectAtIndex:0];
    NSLog(@"path：%@", path);

控制台输出：    


	2015-07-22 00:43:15.803 iOSStrongDemo[1619:556638] /var/mobile/Containers/Data/Application/17300507-4643-4DE7-BC68-E13DB19C8D98/Library


###获取Caches路径

	//获取Caches路径
    NSArray *paths = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES);
    NSString *path = [paths objectAtIndex:0];
    NSLog(@"path：%@", path);

控制台输出：    

	2015-07-22 00:44:31.383 iOSStrongDemo[1626:557083] path：/var/mobile/Containers/Data/Application/1E945B52-E29D-4041-A489-1AA1B11BB960/Library/Caches

###获取tmp路径

    NSString *tmp = NSTemporaryDirectory();
    NSLog(@"tmp：%@", tmp);

控制台输出：    

	2015-07-22 00:46:07.846 iOSStrongDemo[1632:557537] tmp：/private/var/mobile/Containers/Data/Application/4BE02307-1CC5-47E8-BEA8-CEBB7ED5A402/tmp/

以上是iOS开发中获取沙盒路径的方法，也许还有更好的获取方法等待我们去发现。后面我们还会讲到关于在沙盒里进行读写文件操作。
