---
layout: post
title: "从app跳到设置界面"
date: 2015-07-13 23:50:45 +0800
comments: true
categories: iOS开发
keywords: 从app跳到设置界面, app跳设置界面, 设置界面, app, 刚刚在线
description: 看到这张我不由得说一句：又长胖了！好吧，闲话就絮叨这里。今天给大家分享一个小功能：从app跳到设置界面。

---

这次去郑州主要是参加大学同学的婚礼。匆匆而来，匆匆而回。时间安排的好紧，还没有来得及多看两眼我的大好中原。

来参加婚礼的还有其他几位同学，有的真是多年不见。见面之后相互虚寒一番，一起讨论讨论各自的近况。时间飞逝，让这样的相遇变得非常珍贵。

这次的婚礼，别有一番风味。

##第一次在教堂里参加婚礼

我的这位同学，他是一位虔诚的基督徒，他们的婚礼也理所当然的在教堂举行。教堂是一个神圣庄严的地方，虽然我不是一名基督徒，但是我依然怀有一颗虔诚的心。

婚礼还是相当热闹的。有牧师为他们证婚，真的是很幸福。新娘也感动的眼泪都留下来了。

婚礼结束之后，抽空拉着另外一个同学，加上新郎，我们三兄弟合了一张影。这张照片可以发给大家欣赏一下：

![1](http://7xjrlb.com1.z0.glb.clouddn.com/hunli.png)

看到这张我不由得说一句：又长胖了！好吧，闲话就絮叨这里。今天给大家分享一个小功能：从app跳到设置界面。

##从app跳到设置界面

直接上代码：

	 NSURL *url = [NSURL URLWithString:UIApplicationOpenSettingsURLString];
     if ([[UIApplication sharedApplication] canOpenURL:url]) {
         [[UIApplication sharedApplication] openURL:url];
     }
          
上面这段代码要在iOS 8以上的系统使用。一般做智能家居app的，需要到设置界面连接wifi，但是苹果没有提供这样的接口。所以可以退而求其次，跳到系统的设置界面。

想测试的童鞋请下载demo：[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)

##回复关键词获取相应文章

公众号iOS开发：**iOSDevTip**，已经发展一年多的时间，已经积累了很多优秀的文章。也许能帮到一些小忙。

我告诉大家，如何找到这些优秀的文章：

1. 查看历史消息
2. 关键词回复，先回复“m”，获取目录。目录里面详细讲解了，哪些关键词对应哪些文章。

有什么不懂得可以随时联系我，直接在后台回复就可以，也可以加我的微信：**chinaligang**




