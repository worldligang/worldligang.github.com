---
layout: post
title: "自己动手实现一个队列LGQueue"
date: 2015-03-24 17:13:39 +0800
comments: true
categories: iOS开发
keywords: LGQueue, 队列, queue,  dispatch_once, iOS开发, cocoapods, 个人博客, 刚刚在线
description: "自己动手实现一个队列LGQueue"

---

iOS系统没有提供queue容器，如果想用，需要手动自己来写一个：

.h

	//
	//  LGQueue.h
	//  AntsSportApp
	//
	//  Created by ligang on 15/3/24.
	//  Copyright (c) 2015年 ligang. All rights reserved.
	//
	
	#import <Foundation/Foundation.h>
	
	@interface LGQueue : NSObject
	
	@property (nonatomic, strong) NSMutableArray *queue;
	
	@property (nonatomic, readonly) NSInteger count;
	
	- (void)enterQueue:(id)object;
	- (id)exitQueue;
	- (void)clear;
	
	
	
	@end


.m

	//
	//  LGQueue.m
	//  AntsSportApp
	//
	//  Created by ligang on 15/3/24.
	//  Copyright (c) 2015年 ligang. All rights reserved.
	//
	
	#import "LGQueue.h"
	
	@implementation LGQueue
	
	- (id)init
	{
	    if ( self = [super init] ) {
	        
	        self.queue = [[NSMutableArray alloc] init];
	        _count = 0;
	        
	    }
	    return self;
	}
	
	- (void)enterQueue:(id)object
	{
	    [self.queue addObject:object];
	    _count = self.queue.count;
	}
	
	
	- (id)exitQueue
	{
	    if ( self.queue.count == 0 )  return nil;
	    
	    id object = [self.queue objectAtIndex:0];
	    [self.queue removeObjectAtIndex:0];
	    _count = self.queue.count;
	    
	    return object;
	    
	}
	
	
	- (void)clear
	{
	    [self.queue removeAllObjects];
	    _count = 0;
	}
	
	@end
	
	
更多iOS开发相关技术请关注iOS开发微信公众号 iOS开发 ：

	iOSDevTip
	
	