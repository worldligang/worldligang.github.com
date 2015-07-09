---
layout: post
title: "使用UIDataDetectorTypes自动检测电话、网址和邮箱"
date: 2015-07-09 23:43:39 +0800
comments: true
categories: iOS开发
keywords: UIDataDetectorTypes, UIDataDetectorTypes使用, UIDataDetectorTypes用法详解, UIDataDetectorTypes简介, 刚刚在线
description: 使用UIDataDetectorTypes自动检测电话、网址和邮箱。我们先来看看UIDataDetectorTypes有哪些枚举值。

---

支付宝发布最新版本9.0，再一次引发一场撕逼大战。微信说支付宝抄袭了它，支付宝说微信一直都在抄袭自己。在我看来，微信和支付宝都抄袭了对方。对于大佬们的抄袭，我们也是司空见惯了。

支付宝这一次更新，真是赚足了眼球，估计这也是支付宝的推广策略。就是要造势，造势有利于品牌推广。传播速度可想而知，似乎一夜之间都在谈论支付宝和微信。

至于支付宝和微信谁能打过谁，我们拭目以待。

上一篇文章[UIWebView用法详解及代码分享](http://www.superqq.com/blog/2015/07/08/uiwebviewyong-fa-xiang-jie/)里面提到：

	webView.dataDetectorTypes = UIDataDetectorTypePhoneNumber;//自动检测网页上的电话号码，单击可以拨打
	
接下来我们来简单讲一下，使用UIDataDetectorTypes自动检测电话、网址和邮箱。我们先来看看UIDataDetectorTypes有哪些枚举值。

##UIDataDetectorTypes的枚举值

	typedef NS_OPTIONS(NSUInteger, UIDataDetectorTypes) {
	    UIDataDetectorTypePhoneNumber   = 1 << 0,          // Phone number detection
	    UIDataDetectorTypeLink          = 1 << 1,          // URL detection    
	#if __IPHONE_4_0 <= __IPHONE_OS_VERSION_MAX_ALLOWED
	    UIDataDetectorTypeAddress       = 1 << 2,          // Street address detection
	    UIDataDetectorTypeCalendarEvent = 1 << 3,          // Event detection
	#endif    
	
	    UIDataDetectorTypeNone          = 0,               // No detection at all
	    UIDataDetectorTypeAll           = NSUIntegerMax    // All types
	};
	
NS_OPTIONS一般用来定义位移相关操作的枚举值。UIDataDetectorTypeAddress，UIDataDetectorTypeCalendarEvent不在我们考虑的范围了。

UIWebView有dataDetectorTypes属性，UITextView也有dataDetectorTypes属性。我们来UITextView来举例。

##创建UITextView

	UITextView *textView = [[UITextView alloc] initWithFrame:self.view.bounds];
    textView.font = [UIFont systemFontOfSize:20];
    textView.editable = NO;
    textView.text = @"\r\n我的手机号不是： 13888888888 \r\n\r\n"
    "我的博客刚刚在线网址： www.xxxxxx.com \r\n\r\n"
    "我的邮箱： worldligang@163.com \r\n\r\n";
    [self.view addSubview:textView];

运行起来看一下：

![UIDataDetectorTypes1](http://7xjrlb.com1.z0.glb.clouddn.com/UIDataDetectorTypes1.jpeg)

可以看出来就是一个简单的文本，电话号码，网址和邮箱都没有检测出来。下面我们来看看dataDetectorTypes作用。

##UIDataDetectorTypePhoneNumber检测电话

	
    textView.dataDetectorTypes = UIDataDetectorTypePhoneNumber;

设置textView.dataDetectorTypes的属性，即可检测文本中的电话。效果如下所示：

![UIDataDetectorTypes2](http://7xjrlb.com1.z0.glb.clouddn.com/UIDataDetectorTypes2.jpeg)



##UIDataDetectorTypeLink检测网址和邮箱


    textView.dataDetectorTypes = UIDataDetectorTypeLink;

UIDataDetectorTypeLink检测网址和邮箱的。点击网址会跳转到相应的网页，点击邮箱可以调用系统的发邮件。

![UIDataDetectorTypes3](http://7xjrlb.com1.z0.glb.clouddn.com/UIDataDetectorTypes3.jpeg)


##UIDataDetectorTypeAll检测电话、网址和邮箱

    textView.dataDetectorTypes = UIDataDetectorTypeAll;

UIDataDetectorTypeAll可以检测检测电话、网址和邮箱。效果如下：

![UIDataDetectorTypes4](http://7xjrlb.com1.z0.glb.clouddn.com/UIDataDetectorTypes4.jpeg)

专门写了一个iOSStrongDemo以后都在这个iOSStrongDemo里面更新内容。下载地址：[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)
