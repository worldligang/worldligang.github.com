---
layout: post
title: "DKCircleButton"
date: 2015-01-07 14:48:23 +0800
comments: true
categories: 源代码
keywords: DKCircleButton, 源代码, Cocoapods, iPhone, Xcode, Demo, kronik, iOS, iOS开发, cocoapods, 个人博客, 刚刚在线
description: "DKCircleButton"

---

更多iOS开发相关技术请关注iOS开发微信公众号 iOS开发 ：

	iOSDevTip

 <a href="https://github.com/kronik/DKCircleButton.git"target="_blank"title="源代码">源代码</a> 
 
 DKCircleButton
==================

Sources of DKCircleButton and Demo app to show circle button tap effect.

##Download
    $ git clone https://github.com/kronik/DKCircleButton.git
    $ cd DKCircleButton/

##Usage
Please check out the demo project included.
# ![Screenshot](https://raw.github.com/kronik/DKCircleButton/master/example.gif)

### Initialization
Like a regular custom UIButton
``` objective-c
- (void)viewDidLoad {
  [super viewDidLoad];

  DKCircleButton *button1 = [[DKCircleButton alloc] initWithFrame:CGRectMake(0, 0, 90, 90)];
    
  button1.center = CGPointMake(160, 200);
  button1.titleLabel.font = [UIFont systemFontOfSize:22];
}
```

### Toggle animation mode
``` objective-c
  button1.animateTap = NO;
```

##Requirements
Supported build target - iOS 7.x
Earliest supported deployment target - iOS 7.0

##License
DKCircleButton is available under the MIT license. See the LICENSE file for more info.
