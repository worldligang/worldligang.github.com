---
layout: post
title: "解读AFNetworking中Demo的MVC"
date: 2014-11-11 11:00:21 +0800
comments: true
categories: iOS开发
keywords: iOS,AFNetworking, Demo, MVC, iOSDevTip iOS开发, gcd, cocoapods, 队列, 个人博客, 刚刚在线
description: "中文 iOS/Mac 开发博客列表" 

---
Demo的下载地址：<a href=""></a><a href="https://github.com/AFNetworking/AFNetworking.git"target="_blank"title="AFNetworking下载">AFNetworking下载</a>  

打开Demo：

<img src="http://images.90159.com/11/mvc1.jpg">


现实的功能，就是一个简易的微博timeline界面：

<img src="http://images.90159.com/11/mvc2.jpg">

项目预览：

<img src="http://images.90159.com/11/mvc3.jpg">


用到的第三方：AFNetworking 和 UIKit+AFNetworking

功能：用户图像（先显示默认图，在加载图片），用户发的内容，下拉刷新。


<div class="entry-content"><h3>C层：</h3>


发一个请求：（告诉M 要做什么）
	
	- (void)reload:(__unusedid)sender {
	    self.navigationItem.rightBarButtonItem.enabled = NO;
	
	    NSURLSessionTask *task = [PostglobalTimelinePostsWithBlock:^(NSArray *posts, NSError *error) {
	        if (!error) {
	           
	             self.posts = posts;
	            [self.tableViewreloadData];
	        }
	    }];
	
	    [UIAlertViewshowAlertViewForTaskWithErrorOnCompletion:task delegate:nil];
	    [self.refreshControlsetRefreshingWithStateOfTask:task];
	}
	
M 做完之手，产生多个post对象，放到self.post数组里面。C拿到M给的post对象，去更新V

    cell.post = [self.postsobjectAtIndex:(NSUInteger)indexPath.row];


<div class="entry-content"><h3>M层：</h3>


M 收到C的请求：去请求数据，解析，转换成post对象，然后通过block返回给C

	+ (NSURLSessionDataTask *)globalTimelinePostsWithBlock:(void (^)(NSArray *posts, NSError *error))block {
	    return [[AFAppDotNetAPIClientsharedClient] GET:@"stream/0/posts/stream/global"parameters:nilsuccess:^(NSURLSessionDataTask * __unused task, id JSON) {
	        NSArray *postsFromResponse = [JSON valueForKeyPath:@"data"];
	        NSMutableArray *mutablePosts = [NSMutableArrayarrayWithCapacity:[postsFromResponse count]];
	        for (NSDictionary *attributes in postsFromResponse) {
	            Post *post = [[Postalloc] initWithAttributes:attributes];
	            [mutablePosts addObject:post];
	        }
	
	        if (block) {
	            block([NSArrayarrayWithArray:mutablePosts], nil);
	        }
	    } failure:^(NSURLSessionDataTask *__unused task, NSError *error) {
	        if (block) {
	            block([NSArrayarray], error);
	        }
	    }];
	}


<div class="entry-content"><h3>V层：</h3>

V：拿到C给的数据，来展示出来，这一步就是更新UI

	- (void)setPost:(Post *)post {
	    _post = post;
	
	    self.textLabel.text = _post.user.username;
	    self.detailTextLabel.text = _post.text;
	    [self.imageViewsetImageWithURL:_post.user.avatarImageURLplaceholderImage:[UIImageimageNamed:@"profile-image-placeholder"]];
	   
	    [selfsetNeedsLayout];
	}

其中 ：

	    [self.imageViewsetImageWithURL:_post.user.avatarImageURLplaceholderImage:[UIImageimageNamed:@"profile-image-placeholder"]];

这句代码是对UIImageView的扩展，在UIKit+AFNetworking可以直接使用，非常方便。

我的微信公众号 iOS开发 ： 
	
	iOSDevTip

