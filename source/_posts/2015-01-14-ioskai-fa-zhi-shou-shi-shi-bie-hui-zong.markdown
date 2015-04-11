---
layout: post
title: "iOS开发之手势识别汇总"
date: 2015-01-14 10:40:02 +0800
comments: true
categories: iOS开发
keywords: PinchGestureRecognizer,iOS开发, 手势识别, Cocoapods, 手势, RotationGestureRecognizer, TapGestureRecognizer, SwipeGestureRecognizer, LongPressGestureRecognizer, iOS, PanGestureRecognizer, cocoapods, 个人博客, 刚刚在线
description: "iOS开发之手势识别汇总"

---

iOS开发中手势识别有六种：

轻击手势（TapGestureRecognizer）,

轻扫手势 （SwipeGestureRecognizer）,

长按手势（LongPressGestureRecognizer）,

拖动手势（PanGestureRecognizer）,

捏合手势（PinchGestureRecognizer）,

旋转手势（RotationGestureRecognizer）,


###1，轻击手势（TapGestureRecognizer）

    
    UITapGestureRecognizer *tapGesture = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(tapGesture:)];
    tapGesture.numberOfTapsRequired = 1; //点击次数
    tapGesture.numberOfTouchesRequired = 1; //点击手指数
    [self.view addGestureRecognizer:tapGesture];
    
    //轻击手势触发方法
    -(void)tapGesture:(UITapGestureRecognizer *)sender
    {
    	//your code
    }
    

###2，长按手势（LongPressGestureRecognizer）

    UILongPressGestureRecognizer *longPressGesture = [[UILongPressGestureRecognizer alloc] initWithTarget:self action:@selector(longPressGesture:)];
    //设置长按时间
    longPressGesture.minimumPressDuration = 0.5;
    [self.view addGestureRecognizer:longPressGesture];
    
    //长按手势触发方法
    -(void)longPressGesture:(id)sender
    {
	    UILongPressGestureRecognizer *longPress = sender;
	    if (longPress.state == UIGestureRecognizerStateBegan)
	    {
		   //your code
	    }
    }
    
    说明：长按手势的常用状态如下

    开始：UIGestureRecognizerStateBegan

    改变：UIGestureRecognizerStateChanged

    结束：UIGestureRecognizerStateEnded

    取消：UIGestureRecognizerStateCancelled

    失败：UIGestureRecognizerStateFailed

    
###3，轻扫手势（SwipeGestureRecognizer）
    
    UISwipeGestureRecognizer *swipeGesture = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(swipeGesture:)];
    //设置轻扫的方向
    swipeGesture.direction = UISwipeGestureRecognizerDirectionRight; //向右
    [self.view addGestureRecognizer:swipeGesture];

    UISwipeGestureRecognizer *swipeGestureLeft = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(swipeGesture:)];
    //设置轻扫的方向
    swipeGestureLeft.direction = UISwipeGestureRecognizerDirectionLeft; //向左
    [self.view addGestureRecognizer:swipeGestureLeft];
    
   	//轻扫手势触发方法
    -(void)swipeGesture:(id)sender
    {
    
	    UISwipeGestureRecognizer *swipe = sender;
	    
	    if (swipe.direction == UISwipeGestureRecognizerDirectionLeft)
	    {
	    	//向左轻扫
	    }
	    
	    if (swipe.direction == UISwipeGestureRecognizerDirectionRight)
	    {
	    	//向右轻扫
    	}
    }
    
###4，捏合手势（PinchGestureRecognizer）

	UIPinchGestureRecognizer *pinchGesture = [[UIPinchGestureRecognizer alloc] initWithTarget:self action:@selector(pinchGesture:)];
	[self.view addGestureRecognizer:pinchGesture];
	
	    ////捏合手势触发方法
    -(void) pinchGesture:(id)sender
    {
	    UIPinchGestureRecognizer *gesture = sender;
	    //手势改变时
	    if (gesture.state == UIGestureRecognizerStateChanged)
	    {
	   		 //捏合手势中scale属性记录的缩放比例
	    	_imageView.transform = CGAffineTransformMakeScale(gesture.scale, gesture.scale);
	    }
	    
	    //结束后恢复
	    if(gesture.state==UIGestureRecognizerStateEnded)
	    {
	    	[UIView animateWithDuration:0.5 animations:^{
	    		_imageView.transform = CGAffineTransformIdentity;//取消一切形变
	    	}];
	    }
    }
    
###5，拖动手势（PanGestureRecognizer）

	UIPanGestureRecognizer *panGesture = [[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(panGesture:)];
	[self.view addGestureRecognizer:panGesture];
	
	//拖动手势触发方法
    -(void) panGesture:(id)sender
    {
    	UIPanGestureRecognizer *panGesture = sender;
    	CGPoint movePoint = [panGesture translationInView:self.view];
    	//your code
    }
    
###6，旋转手势（RotationGestureRecognizer）

	UIRotationGestureRecognizer *rotationGesture = [[UIRotationGestureRecognizer alloc] initWithTarget:self action:@selector(rotationGesture:)];
	[self.view addGestureRecognizer:rotationGesture];
	
	//旋转手势触发方法
	-(void)rotationGesture:(id)sender
	{
		UIRotationGestureRecognizer *gesture = sender;
		if (gesture.state==UIGestureRecognizerStateChanged)
		{
			_imageView.transform=CGAffineTransformMakeRotation(gesture.rotation);
		}
		if(gesture.state==UIGestureRecognizerStateEnded)
		{
			[UIView animateWithDuration:1 animations:^{
				_imageView.transform=CGAffineTransformIdentity;//取消形变
			}];
		}
	}
	
更多iOS开发相关技术请关注iOS开发微信公众号 iOS开发 ：

	iOSDevTip