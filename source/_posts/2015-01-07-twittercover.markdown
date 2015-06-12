---
layout: post
title: "TwitterCover"
date: 2015-01-07 13:20:17 +0800
comments: true
categories: 源代码
keywords: TwitterCover, 源代码, Cocoapods, UIScrollView, Xcode, parallax, 客户端, iOS, iOS开发, cocoapods, 个人博客, 刚刚在线
description: "TwitterCover"

---

更多iOS开发相关技术请关注iOS开发微信公众号 iOS开发 ：

	iOSDevTip

 <a href="https://github.com/cyndibaby905/TwitterCover.git"target="_blank"title="源代码">源代码</a> 
 
 ## TwitterCover ##

TwitterCover is a parallax top view with real time blur effect to any UIScrollView, inspired by Twitter for iOS.

Completely created using UIKit framework.

Easy to drop into your project.

You can add this feature to your own project, `TwitterCover` is easy-to-use. 


## Requirements ##

TwitterCover requires Xcode 5, targeting either iOS 5.0 and above, ARC-enabled.


## How to use ##
	
Drag UIScrollView+TwitterCover.h amd UIScrollView+TwitterCover.m files to your project. 

No other frameworks required.

    #import "UIScrollView+TwitterCover.h"

    UIScrollView *scrollView = [[UIScrollView alloc] initWithFrame:self.view.bounds];
    [scrollView addTwitterCoverWithImage:[UIImage imageNamed:@"cover.png"]];

And do not forget to remove it in your dealloc method, otherwise memory leaks:

    [scrollView removeTwitterCoverView];    

## How it looks ##

<img src="https://raw.github.com/cyndibaby905/TwitterCover/master/TwitterCover.gif" /></a>


## Lincense ##

`TwitterCover` is available under the MIT license. See the LICENSE file for more info.


**更多源代码：**


[DKCircleButton](http://www.superqq.com/blog/2015/01/07/dkcirclebutton/)

[VVDocumenter-Xcode](http://www.superqq.com/blog/2015/01/06/vvdocumenter-xcode/)

[HackerNews](http://www.superqq.com/blog/2015/01/07/hackernews/)

[GPUImage](http://www.superqq.com/blog/2015/01/06/gpuimage/)
