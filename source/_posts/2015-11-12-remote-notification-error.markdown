---
layout: post
title:  远程推送控制台提示Add “Remote-notification”...解决方案 
date: 2015-11-12 18:21:17 +0800
comments: true
categories: iOS开发
keywords: remote-notification, UIBackgroundModes,多线程 , iOS开发, 刚刚在线
description: add "remote-notification" to the list of your supported UIBackgroundModes in your Info.plist.解决方案

---


做推送的童鞋可能会遇到这样的问题，控制台会打印一句话：

You've implemented -[<UIApplicationDelegate> application:didReceiveRemoteNotification:fetchCompletionHandler:], but you still need to add "remote-notification" to the list of your supported UIBackgroundModes in your Info.plist.


解决办法：

![error1](http://images.90159.com/11/error1.png)

这个时候再去看看Info.plist文件：

![error2](http://images.90159.com/11/error2.png)

再次运行，问题就解决啦！