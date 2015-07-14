---
layout: post
title: "Xcode6.4注册URL Scheme步骤详解"
date: 2015-07-14 23:34:16 +0800
comments: true
categories: iOS开发
keywords: URL Scheme, URL Identifier, Xcode6.4, Scheme, 刚刚在线
description: 我们都知道苹果手机中的APP都有一个沙盒，APP就是一个信息孤岛，相互是不可以进行通信的。但是iOS的APP可以注册自己的URL Scheme，URL Scheme是为方便app之间互相调用而设计的。我们可以通过系统的OpenURL来打开该app，并可以传递一些参数。

---
好消息：微信团队邀请我的iOS开发（id：**iOSDevTip**）微信公众号开通赞赏功能，以后我就可以光明正大的通过文章要饭啦。你的赞赏就是我最大的动力。

这是今天下午15：30收到的邀请：

![zanshang1](http://7xjrlb.com1.z0.glb.clouddn.com/zanshang.png)

##微信赞赏功能

为什么推出赞赏功能？微信公众号自2012年推出以来，一直成爆发式的增长。微信推出赞赏功能最重要的目的就是为了鼓励原创，鼓励创新。

之前，原创功能、赞赏功能和评论功能是一起邀请开通的。现在微信团队分三次邀请。现在我的iOS开发公众号，已经被邀请开通了原创和赞赏功能，下一步争取开通评论功能。

如果开通评论功能，我就可以更方便的和大家交流。一经邀请，我会第一时间告诉大家的。好消息，大家也知道啦。接下来继续回到正题，我们来谈谈iOS中的URL Scheme。

##URL Scheme的作用

我们都知道苹果手机中的APP都有一个沙盒，APP就是一个信息孤岛，相互是不可以进行通信的。但是iOS的APP可以注册自己的URL Scheme，URL Scheme是为方便app之间互相调用而设计的。我们可以通过系统的OpenURL来打开该app，并可以传递一些参数。

例如：你在Safari里输入www.alipay.com，就可以直接打开你的支付宝app，前提是你的手机装了[支付宝](http://www.superqq.com/blog/2015/07/10/zhi-fu-bao-he-wei-xin-zai-chao-xi-shang-shui-geng-sheng-%5B%3F%5D-chou/)。如果你没有装支付宝，应该显示的是支付宝下载界面，点击会跳到AppStore的支付宝下载界面。

URL Scheme必须能唯一标识一个APP，如果你设置的URL Scheme与别的APP的URL Scheme冲突时，你的APP不一定会被启动起来。因为当你的APP在安装的时候，系统里面已经注册了你的URL Scheme。

一般情况下，是会调用先安装的app。但是iOS的系统app的URL Scheme肯定是最高的。所以我们定义URL Scheme的时候，尽量避开系统app已经定义过的URL Scheme。

##注册URL Scheme

###1.在info.plist里添加URL types

每一个项目里面都会有一个info.plist配置文件。找到info.plist，右键选择Add Row，然后选择URL types。如图所示：

![url1](http://7xjrlb.com1.z0.glb.clouddn.com/url1.jpg)

###2.添加URL Schemes
添加完URL types，点击展开。右键选择Add Row，添加URL Schemes：

![url2](http://7xjrlb.com1.z0.glb.clouddn.com/url2.jpg)

###3.设置URL Schemes

设置URL Schemes为iOSDevTip

![url3](http://7xjrlb.com1.z0.glb.clouddn.com/url3.jpg)

###4.设置URL Identifier

URL Identifier是自定义的 URL scheme 的名字，一般采用反转域名的方法保证该名字的唯一性，比如 com.iOSStrongDemo.www

![url4](http://7xjrlb.com1.z0.glb.clouddn.com/url4.jpg)

##添加成功启动提示

为了方便测试，我们在AppDelegate里面添加一个UIAlertView，当app被成功打开时，会提出提示：

	- (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL*)url
	{
	    // 接受传过来的参数
	    NSString *text = [[url host] stringByReplacingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
	    UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"打开啦"
	                                           message:text
	                                          delegate:nil
	                                 cancelButtonTitle:@"OK"
	                                 otherButtonTitles:nil];
	    [alertView show];
	    return YES;
	}

##Safari启动自定义的URL Schemes APP

既然已经配置好URL Schemes，那么我们可以来款速测试一下，我们设置的URL Schemes是否有效。打开Safari，在地址栏里输入：iOSDevTip://

![url6](http://7xjrlb.com1.z0.glb.clouddn.com/url6.png)

果然成功打开：

![url5](http://7xjrlb.com1.z0.glb.clouddn.com/url5.png)

也可以在地址栏中输入：iOSDevTip://com.iOSStrongDemo.www。也是可以打开注册了URL Schemes的APP的。

##通过另一个APP启动注册了URL Schemes的APP

	 NSString *url = @"iOSDevTip://";
	//    NSString *url = @"iOSDevTip://com.iOSStrongDemo.www";
    if ([[UIApplication sharedApplication]
         canOpenURL:[NSURL URLWithString:url]])
    {
        [[UIApplication sharedApplication] openURL:[NSURL URLWithString:url]];
    }
    else
    {
        NSLog(@"can not open URL scheme iOSDevTip");
    }

打开注册iOSDevTip的APP格式为: URL Scheme://URL identifier，直接调用URL Scheme也可打开程序, URL identifier是可选的。

注册URL Scheme就是这么简单。下一篇会分享如何通过URL Scheme传递参数。demo下载地址：[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)