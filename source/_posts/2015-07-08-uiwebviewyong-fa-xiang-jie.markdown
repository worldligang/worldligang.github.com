---
layout: post
title: "UIWebView用法详解及代码分享"
date: 2015-07-08 23:52:34 +0800
comments: true
categories: iOS开发
keywords: UIWebView, UIWebView使用, UIWebView用法详解, UIWebView简介, 刚刚在线
description: UIWebView是iOS内置的浏览器控件，可以浏览网页、打开文档等 能够加载html/htm、pdf、docx、txt等格式的文件.

---

今天加入一个QQ群，也算是一个圈子，群主的要求是满足他的三个条件。经过与群主的沟通，终于得到通过，顺利加入。

群主是一个93年的小伙，而且是河南的老乡。没想到小伙子这么有号召力。我也是偶然在我的空间发现他的，看他每天发的文章，还挺有意思。

**90后的小伙子都这么厉害了，有什么理由不努力奋斗呢！**多加入以下圈子，认识一下身边的牛人。说不定会有意外的收获。

今天我们来详细UIWebView用法。UIWebView是iOS内置的浏览器控件，可以浏览网页、打开文档等 能够加载html/htm、pdf、docx、txt等格式的文件.

用UIWebView我们就可以做一个简易的浏览器。效果如下：

![1](http://7xjrlb.com1.z0.glb.clouddn.com/webview1.png)

 
##创建UIWebView

首先创建UIWebView，这个不难，设置frame，然后添加到self.view上即可：
 
	UIWebView *webView = [[UIWebView alloc] initWithFrame:self.view.bounds];
	webView.scalesPageToFit = YES;//自动对页面进行缩放以适应屏幕
	[self.view addSubview:webView];
    
##UIWebView加载网络地址

用UIWebView来加载网络地址，那我的博客：**[http://www.superqq.com](http://www.superqq.com)** 来举例。代码如下：
 
	NSURLRequest *request =[NSURLRequest requestWithURL:[NSURL URLWithString:@"http://www.superqq.com"]];
	[self.view addSubview:webView];
	[webView loadRequest:request];
		
##UIWebView加载本地资源
	 
###通过NSURLRequest加载
 	
    NSString *path = [[NSBundle mainBundle] pathForResource:@"swift" ofType:@"html"];
    NSURL* url = [NSURL   fileURLWithPath:path];//创建URL
    NSURLRequest* request = [NSURLRequest requestWithURL:url];//创建NSURLRequest
    [webView loadRequest:request];//加载
    
###通过NSString加载

UIWebView 还支持将一个NSString对象作为源来加载。你可以为其提供一个基础URL，来指导UIWebView对象如何跟随链接和加载远程资源： 
 
	NSString *path = [[NSBundle mainBundle] pathForResource:@"index" ofType:@"html"];
    NSString *htmlString = [NSString stringWithContentsOfFile:path encoding:NSUTF8StringEncoding error:nil];
    [webView loadHTMLString:htmlString baseURL:[NSURL URLWithString:path]];

##自动检测电话

设置这个属性，如果在加载的网页中遇到电话号码，直接单击就可以拨打，非常方便：

	webView.dataDetectorTypes = UIDataDetectorTypePhoneNumber;//自动检测网页上的电话号码，单击可以拨打
	

##设置代理和代理方法

UIWebView有五个代理方法，我们可以在不同的代理方法中，完成不同的需求。

	//设置代理
	webView.delegate = self;
	
	//代理方法
	- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType
	{
	    //返回YES，进行加载。通过UIWebViewNavigationType可以得到请求发起的原因
	    return YES;
	}
	
	- (void)webViewDidStartLoad:(UIWebView *)webView
	{
	    //开始加载，可以加上风火轮（也叫菊花）
	}
	
	- (void)webViewDidFinishLoad:(UIWebView *)webView
	{
	    //完成加载
	}
	
	- (void)webView:(UIWebView *)webView didFailLoadWithError:(NSError *)error
	{
	    //加载出错
	}
	

##创建加载他、停止、后退、前进按钮

创建四个按钮，分别是加载按钮、停止按钮、返回按钮、前进按钮：
	
	NSArray *array = [NSArray arrayWithObjects:@"加载",@"停止",@"返回",@"前进", nil];
	    int far = (SCREEN.width - 200)/5;
	    for (int i = 0; i < array.count; i++) {
	        UIButton *button = [[UIButton alloc] init];
	        button.frame = CGRectMake( (i + 1) * far + 50 * i, SCREEN.height - 60, 50, 40);
	        button.tag = 200 + i;
	        [button addTarget:self action:@selector(actionClick:) forControlEvents:UIControlEventTouchUpInside];
	        [button setTitleColor:[UIColor blueColor] forState:UIControlStateNormal];
	        [button setBackgroundColor:[UIColor orangeColor]];
	        [button setTitle:array[i] forState:UIControlStateNormal];
	        [self.view addSubview:button];
	    }

##前进后退

给四个按钮绑定事件，根据tag值来区分每一个点击事件：
		
	- (void)actionClick:(UIButton *)button
	{
	    switch (button.tag) {
	        case 200:
	        {
	            [self.webView reload];//加载
	        }
	            break;
	        case 201:
	        {
	            [self.webView stopLoading];//停止加载
	        }
	            break;
	        case 202:
	        {
	            [self.webView goBack];//返回
	        }
	            break;
	        case 203:
	        {
	            [self.webView goForward];//前进
	        }
	            break;
	        default:
	            break;
	    }
	}

大功告成，再来看看效果：

![2](http://7xjrlb.com1.z0.glb.clouddn.com/webview2.png)

以上分享的内容我做了一个demo，需要的话可以点击这里下载：[UIWebView用法详解](https://github.com/worldligang/UIWebViewDemo)
