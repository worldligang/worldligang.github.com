---
layout: post
title: " iOS协议里面可以定义成员变量和属性吗？"
date: 2015-05-22 11:08:18 +0800
comments: true
categories: iOS开发
keywords: iOS, 协议, protocol,  成员变量, 属性, 获取当前位置信息, iOS协议, iOS开发, cocoapods, 个人博客, 刚刚在线
description: "iOS协议里面可以定义成员变量和属性吗？"

---

>关注iOS开发：iOSDevTip

在CSDN上看到有人提的问题，觉得很有价值，先看代码：

   
    @protocol CAMediaTiming
    
    /* The begin time of the object, in relation to its parent object, if
     * applicable. Defaults to 0. */
    
    @property CFTimeInterval beginTime;
    
    /* The basic duration of the object. Defaults to 0. */
    
    @property CFTimeInterval duration;
    
    /* The rate of the layer. Used to scale parent time to local time, e.g.
     * if rate is 2, local time progresses twice as fast as parent time.
     * Defaults to 1. */
    
    @property float speed;
    
    /* Additional offset in active local time. i.e. to convert from parent
     * time tp to active local time t: t = (tp - begin) * speed + offset.
     * One use of this is to "pause" a layer by setting `speed' to zero and
     * `offset' to a suitable value. Defaults to 0. */
    
    @property CFTimeInterval timeOffset;
    
    /* The repeat count of the object. May be fractional. Defaults to 0. */
    
    @property float repeatCount;
    
    /* The repeat duration of the object. Defaults to 0. */
    
    @property CFTimeInterval repeatDuration;
    
    /* When true, the object plays backwards after playing forwards. Defaults
     * to NO. */
    
    @property BOOL autoreverses;
    
    /* Defines how the timed object behaves outside its active duration.
     * Local time may be clamped to either end of the active duration, or
     * the element may be removed from the presentation. The legal values
     * are `backwards', `forwards', `both' and `removed'. Defaults to
     * `removed'. */
    
    @property(copy) NSString *fillMode;
    
    @end

###问题：

上面那个协议时苹果自带的，为什么里面定义了成员变量呢？             

###网友回答：

这只是在头文件中声明，编译器是不会自动生成实例变量的，也就是说你可以用一个对象去实现这个协议，但是你不自己存储的话实例变量的话，访问同样会崩溃：
self.fillModel = @"test"; // unrecognized selector        

###追问

你好，也就是说我需要这样：

	@interface MyClass : NSObject < CAMediaTiming >
	@property CFTimeInterval beginTime;
	@end
	
这样才可以么？
也就是@protocol能声明，不能定义变量么？ 

###网友回答

协议声明出来的是方法，虽然看起来是属性，其实只有 getter、setter 这两个方法，内部是没有实例变量的，并且你不能去重定义它，只能在 getter、setter 里处理 
​