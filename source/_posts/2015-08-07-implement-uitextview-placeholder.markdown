---
layout: post
title: "实现placeholder属性的UITextView"
date: 2015-08-07 00:27:43 +0800
comments: true
categories: iOS开发
keywords: UITextField, placeholder, GGPlaceholderTextView, iOS开发, 刚刚在线
description: 我们都知道iOS开发中的UITextField有个placeholder属性，placeholder可以很方便引导用户输入。但是UITextView却没有placeholder属性。

---

iOS开发微信公众号上的iOS技术文章，都将发布在[刚刚在线](http://www.superqq.com/)。所以大家可以到我的网站上查看之前的文章，也可以在底部的精彩文章查看历史文章。

上一篇文章[UITextView实现placeholder的猥琐做法](http://www.superqq.com/blog/2015/08/06/uitextview-placeholder-practice/)讲了关于`UITextView`的`placeholder`实现原理。大家都一致认为做法比较猥琐。

<!--more-->

那么，这一次我将简单的封装一个`UITextView`。暂且取名叫`GGPlaceholderTextView`，GG前缀看着有点任性的哈。

##GGPlaceholderTextView简介

`GGPlaceholderTextView`也是对`text`操作，具体逻辑如下：

1. 继承`UITextView`，并设置`placeholder`属性
2. 注册开始编辑和结束编辑通知，然后对`text`做相应的操作
3. 通过`UIApplicationWillTerminateNotification`通知，在APP退出的时候移除通知。


我把`GGPlaceholderTextView`写在下面。不过，微信里看代码还是不太方便，我已经把代码push到:[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)。你可以下载下来。

##GGPlaceholderTextView.h

	#import <UIKit/UIKit.h>
	
	@interface GGPlaceholderTextView : UITextView
	@property(nonatomic, strong) NSString *placeholder;
	
	@end

定义`placeholder`属性，类似于`UITextField`。

##GGPlaceholderTextView.m

	#import "GGPlaceholderTextView.h"
	
	@implementation GGPlaceholderTextView
	
	- (id)initWithFrame:(CGRect)frame {
	    if (self = [super initWithFrame:frame]) {
	        [self addObserver];
	    }
	    return self;
	}
	
	- (id)init {
	    if (self = [super init]) {
	        [self addObserver];
	    }
	    return self;
	}
	
	- (void)setPlaceholder:(NSString *)placeholder
	{
	    _placeholder = placeholder;
	    self.text = placeholder;
	    self.textColor = [UIColor grayColor];
	}
	
	-(void)addObserver
	{
	    //注册通知
	    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(didBeginEditing:) name:UITextViewTextDidBeginEditingNotification object:self];
	    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(didEndEditing:) name:UITextViewTextDidEndEditingNotification object:self];
	    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(terminate:) name:UIApplicationWillTerminateNotification object:[UIApplication sharedApplication]];
	}
	
	- (void)terminate:(NSNotification *)notification {
	    //移除通知
	    [[NSNotificationCenter defaultCenter] removeObserver:self];
	}
	
	- (void)didBeginEditing:(NSNotification *)notification {
	    if ([self.text isEqualToString:self.placeholder]) {
	        self.text = @"";
	        self.textColor = [UIColor blackColor];
	    }
	}
	
	- (void)didEndEditing:(NSNotification *)notification {
	    if (self.text.length<1) {
	        self.text = self.placeholder;
	        self.textColor = [UIColor grayColor];
	    }
	}
	
	@end

以上就是关于`GGPlaceholderTextView`的实现，如果你有类似需求，直接拿去用吧！具体用法请往下看。

##实践

    GGPlaceholderTextView *textView = [[GGPlaceholderTextView alloc] initWithFrame:CGRectMake(0, 64, SCREEN.width , 200)];
    textView.backgroundColor = [UIColor whiteColor];
    textView.placeholder = @"关注微信公众号iOS开发：iOSDevTip";
    [self.view addSubview:textView];

经过封装后的`GGPlaceholderTextView`，使用起来是不是跟`UITextField`非常相似。当然，我封装的比较简单，github上也有一些朋友封装带`placeholder`属性的`UITextView`。比如：TextViewPlaceholder。感兴趣的童鞋可以去试用一下。

**广告时间**

你的分享就是对我最大的支持，希望你能把iOS开发推荐给你的朋友，不甚感激！