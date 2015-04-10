---
layout: post
title: "iOS开发之UISearchBar初探"
date: 2015-01-19 09:56:52 +0800
comments: true
categories: iOS开发
keywords: UISearchBar, LSSearchBar, placeholder, 修改placeholder的颜色, 修改UISearchBar,  iOS开发, gcd, cocoapods, 队列, 个人博客, 刚刚在线
description: "iOS开发之UISearchBar初探"


---

UISearchBar也是iOS开发常用控件之一，点进去看看里面的属性barStyle、text、placeholder等等。但是这些属性显然不足矣满足我们的开发需求。比如：修改placeholder的颜色、修改UISearchBar上面的UITextfield的背景颜色、修改UITextfield上面的照片等等。

为了实现上述的需求，最好写一个UISearchBar的子类就叫LSSearchBar吧

LSSearchBar.h如下：

	#import <UIKit/UIKit.h>

	@interface LSSearchBar : UISearchBar

	@end

LSSearchBar.m如下：

	#import "LSSearchBar.h"

	@implementation LSSearchBar
	
	- (void)layoutSubviews {
	    
	    [super layoutSubviews];
	
		//通过遍历self.subviews找到searchField
	    UITextField *searchField;
	    NSUInteger numViews = [self.subviews count];
	    for(int i = 0; i < numViews; i++) {
	        if([[self.subviews objectAtIndex:i] isKindOfClass:[UITextField class]]) {
	            searchField = [self.subviews objectAtIndex:i];
	        }
	    }
	    
	   	//如果上述方法找不到searchField，那就试试下面的方法吧
	    
	    if (searchField ==  nil) {
	        NSArray *arraySub = [self subviews];
	        UIView *viewSelf = [arraySub objectAtIndex:0];
	        NSArray *arrayView = [viewSelf subviews];
	        for(int i = 0; i < arrayView.count; i++) {
	            if([[arrayView objectAtIndex:i] isKindOfClass:[UITextField class]]) {
	                searchField = [arrayView objectAtIndex:i];
	            }
	        }
	    }
	    
	    
		if(!(searchField == nil)) {
	        //设置颜色
	        searchField.textColor = [UIColor whiteColor];
	        
	        //设置背景颜色
	        [searchField setBackground: [UIImage imageNamed:@"searchbar"] ];
	        [searchField setBorderStyle:UITextBorderStyleNone];
	        
	        //设置placeholder的颜色
	        [searchField setValue:[UIColor whiteColor] forKeyPath:@"_placeholderLabel.textColor"];
	        
	        //设置searchField上的照片
	        UIImage *image = [UIImage imageNamed:@"search"];
	        UIImageView *iView = [[UIImageView alloc] initWithImage:image];
	        iView.frame = CGRectMake(0, 0, 15, 15);
	        searchField.leftView = iView;
	    }
	    
	}
	
	@end


更多iOS开发相关技术请关注iOS开发微信公众号 iOS开发 ：

	iOSDevTip