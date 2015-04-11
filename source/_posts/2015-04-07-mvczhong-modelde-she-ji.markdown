---
layout: post
title: "MVC中Model的设计"
date: 2015-04-07 09:56:12 +0800
comments: true
categories: iOS开发
keywords: MVC, Model, git,  基类, LGObject, BaseViewController, iOS开发, cocoapods, 个人博客, 刚刚在线
description: "MVC中Model的设计"

---

一般在定义ViewController的时候，都习惯定义一个BaseViewController作为基类（即父类）。这样做的好处是：把公共的属性提出来放在BaseViewController中，试代码结构更清晰。同样，设计Model的时候，也可以定义一个基类。

比如：有一个房子，房子里面有一个电视和许多个桌子。电视和桌子分别有价格和型号。

###定义基类LGObject

LGObject.h：
    
	 //
	//  LGObject.h
	//  LGAlertViewDemo
	//
	//  Created by apple on 15/4/6.
	//  Copyright (c) 2015年 LiGang. All rights reserved.
	//
	
	#import <Foundation/Foundation.h>
	
	@interface LGObject : NSObject
	
	@property (readonly, nonatomic) id data;
	
	- (id)initWithData:(id)data;
	- (BOOL)isEmpty;
	
	@end

LGObject.m：

	//
	//  LGObject.m
	//  LGAlertViewDemo
	//
	//  Created by apple on 15/4/6.
	//  Copyright (c) 2015年 LiGang. All rights reserved.
	//
	
	#import "LGObject.h"
	
	@implementation LGObject
	
	- (id)initWithData:(id)data {
	    self = [super init];
	    if(self) {
	        _data = nil;
	        if(data != nil) _data = data;
	    }
	    return self;
	}
	
	- (BOOL)isEmpty
	{
	    return _data == nil ? YES : NO;
	}
	
	@end

###定义房子LGHouse、电视LGVideo、桌子LGDesk

电视LGVideo、桌子LGDesk可以放在LGHouse里面。多个桌子需要定义一个数组：arrayDesk。

LGHouse.h：

	//
	//  LGHouse.h
	//  LGAlertViewDemo
	//
	//  Created by apple on 15/4/6.
	//  Copyright (c) 2015年 LiGang. All rights reserved.
	//
	
	#import "LGObject.h"
	
	@class LGVideo;
	@class LGDesk;
	
	@interface LGHouse : LGObject
	
	@property (nonatomic, readonly) LGVideo  *video;     //电视
	@property (nonatomic, readonly) NSArray  *arrayDesk; //桌子
	
	@end
	
	@interface LGVideo : LGObject
	
	@property (nonatomic, assign)   float      money;//价格
	@property (nonatomic, readonly) NSString  *type;//型号
	
	@end
	
	@interface LGDesk : LGObject
	
	@property (nonatomic, assign)   float      money;//价格
	@property (nonatomic, readonly) NSString  *type;//型号
	
	@end


LGHouse.m：

	//
	//  LGHouse.m
	//  LGAlertViewDemo
	//
	//  Created by apple on 15/4/6.
	//  Copyright (c) 2015年 LiGang. All rights reserved.
	//
	
	#import "LGHouse.h"
	
	@implementation LGHouse
	
	- (LGVideo *)video {
	    return [[LGVideo alloc] initWithData:[self.data objectForKey:@"video"]];
	}
	
	- (NSArray *)arrayDesk {
	    NSMutableArray *arrayDesk = [NSMutableArray array];
	    for (NSDictionary *dickDesk in [self.data objectForKey:@"desks"]) {
	        [arrayDesk addObject:[[LGDesk alloc] initWithData:dickDesk]];
	    }
	    return arrayDesk;
	}
	
	@end
	
	@implementation LGVideo
	
	- (float)money {
	    return [[self.data objectForKey:@"money"] floatValue];
	}
	
	- (NSString *)type {
	    return [self.data objectForKey:@"type"];
	}
	
	@end
	
	@implementation LGDesk
	
	- (float)money {
	    return [[self.data objectForKey:@"money"] floatValue];
	}
	
	- (NSString *)type {
	    return [self.data objectForKey:@"type"];
	}
	
	@end

以上是我对Model设计的简单理解，如果你有更好的想法，欢迎告诉我。
     





