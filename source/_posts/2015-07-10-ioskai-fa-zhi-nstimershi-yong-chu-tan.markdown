---
layout: post
title: "iOS开发之NSTimer使用初探"
date: 2015-07-10 23:21:44 +0800
comments: true
categories: iOS开发
keywords: NSTimer, NSTimer使用, NSTimer用法详解, NSTimer简介, 刚刚在线
description: 开始我以为是NSTimer运行的runloop有问题，最后发现是别的地方导致这个bug的。不过正好，我们来一起了解一下NSTimer。

---
这个周日，我的大学同学结婚。他们的婚礼选在郑州举办。他不是郑州的，他女朋友也不是郑州的。但是他们是在郑州相识相爱，最终走到一起的，所以他们选择在郑州举办婚礼。

这几年大家都在各自忙碌着，很少有见面的机会。不过经常会电话联系，相互了解一下情况。

作为兄弟，我打心底为他们高兴，为他们祝福。我也会到现场参加，估计现在我已经坐上开发郑州的动车了。

我喜欢走出去的感觉，很喜欢坐车，去各个地方，哪怕是路过。**感觉走的越远，离心越近。**辞职去旅行也许很快就可以实现。

最近我在改一个bug，在一个UIViewController上有一个定时器，在执行轮播图片。push到别的UIViewController的时候，暂停定时器（NSTimer）；pop回来的时候，开启定时器（NSTimer）。这个bug是：push到别的UIViewController暂定定时器（NSTimer），有时候有作用，有时候没有作用。

开始我以为是NSTimer运行的run loop有问题，最后发现是别的地方导致这个bug的。不过正好，我们来一起了解一下NSTimer。

##创建一个定时器（NSTimer）

	- (void)viewDidLoad {
	    [super viewDidLoad];
    	[NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(actionTimer:) userInfo:nil repeats:YES];
	}
	
	- (void)actionTimer:(NSTimer *)timer
	{
	
	}

NSTimer默认运行在default mode下，default mode几乎包括所有输入源(除NSConnection) NSDefaultRunLoopMode模式。

actionTimer方法会每隔1s中被调用一次。NSTimer使用起来是不是非常简单。这是NSTimer比较初级的应用。

##当主界面被滑动时NSTimer失效了

主界面被滑动是什么意思呢？就是说主界面有UITableView或者UIScrollView，滑动UITableView或者UIScrollView。这个时候NSTimer失效了。

我们来写一个demo，在一个有UITableView的UIViewController上启动定时器，每1s数字加1，并将这个数字显示在UILabel上面.

	- (void)viewDidLoad {
	    [super viewDidLoad];
    	[NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(actionTimer:) userInfo:nil repeats:YES];
	}
	
	- (void)actionTimer:(NSTimer *)timer
	{
	    self.number++;
	    self.label.text = [NSString stringWithFormat:@"%d",self.number];
	    NSLog(@"%d",self.number);
	}

关于UITableView和UILabel的创建我省去了。详细的代码可以点击这里下载：[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)，iOSStrongDemo我会不断更新，大家在github上star一下。

这样当用户在拖动UITableView处于UITrackingRunLoopMode时，NSTimer就失效了，不能fire。self.label上的数字也就无法更新。

![nstimer1](http://7xjrlb.com1.z0.glb.clouddn.com/timer1.gif)

##修改NSTimer的run loop

解决方法就是将其加入到UITrackingRunLoopMode模式或NSRunLoopCommonModes模式中。


	[[NSRunLoop currentRunLoop] addTimer:timer forMode:UITrackingRunLoopMode];

或者

	[[NSRunLoop currentRunLoop] addTimer:timer forMode:NSRunLoopCommonModes];

NSRunLoopCommonModes：是一个模式集合，当绑定一个事件源到这个模式集合的时候就相当于绑定到了集合内的每一个模式。

![nstimer1](http://7xjrlb.com1.z0.glb.clouddn.com/timer2.gif)


代码下载地址：：[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)
