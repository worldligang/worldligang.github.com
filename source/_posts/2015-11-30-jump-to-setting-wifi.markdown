---
layout: post
title: iOS开发之如何跳到系统设置里的WiFi界面
date: 2015-11-30 22:14:58 +0800
comments: true
categories: iOS开发
keywords: WiFi, 系统设置,系统设置里的WiFi界面 , iOS开发, 刚刚在线
description: 之前以为，苹果不支持直接从应用跳到系统设置里的WiFi界面。后来发现，这个小功能是可以实现的，而且实现起来并不麻烦。让我们一起来看看吧！

---

之前以为，苹果不支持直接从应用跳到系统设置里的WiFi界面。后来发现，这个小功能是可以实现的，而且实现起来并不麻烦。让我们一起来看看吧！

##需求

从应用跳到系统设置里的WiFi界面有这个需求存在吗？答案是肯定的。比如以下两个例子：

在没有网的状态下，你可能想提醒用户去设置界面连接WiFi。如果不能跳到WiFi界面，只能在APP里面做出文字提示。这样很多小白用户可能不会看提示，只会觉得APP没有做好。

还有一种情况，做智能家居的APP，智能硬件设备自带WiFi（局域网）。如果用户没有连接设备的WiFi进入APP时，需要提示用户去设置界面连接WiFi。

以上这两种情况只是举个例子，这个小功能的用处还是很多的，大家可以自行探索。

<!--more-->

##实现

###info里面设置

在项目中的info.plist中添加 URL types 并设置一项URL Schemes为prefs，如下图：

![wifi1](http://images.90159.com/12/wifi1.png)


###实现代码

	NSURL *url = [NSURL URLWithString:@"prefs:root=WIFI"];
    if ([[UIApplication sharedApplication] canOpenURL:url])
    {
        [[UIApplication sharedApplication] openURL:url];
    }

##代码已上传iOSStrongDemo

代码已经push到[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)，大家可以clone下来测试一下。

我在这里抛砖迎玉，大家还想跳到系统设置的什么界面，可以评论留言告诉我，或者大家动手来实现更多的跳转功能！