---
layout: post
title: CoreMotion框架中加速度计使用简明教程
date: 2015-08-10 00:37:33 +0800
comments: true
categories: iOS开发
keywords: UIAccelerometer, 加速度计, CoreMotion, iOS开发, 刚刚在线
description: 在iOS4之前加速度计是由UIAccelerometer来采集数据的，现在都是用CoreMotion来处理。两种使用都比较简单，相对来说UIAccelerometer更简单一些。本文将分享两者的用法。

---
##前言

之前有人通过iPhone的加速度计做出一个手机防盗APP，而且正是通过这个防盗APP拿到天使投资。从此以后华丽转身，公司发展的有声有色。虽然这个项目最后不怎么样，但是，确实是一个不错的想法。

也许你不知道，每一台iPhone都内置的有加速度计。这样当用户操作手机的的时候，比如旋转手机，内置的加速度计就会做出反应。

在iOS4之前加速度计是由`UIAccelerometer`来采集数据的，现在都是用`CoreMotion`来处理。两种使用都比较简单，相对来说`UIAccelerometer`更简单一些。本文将分享两者的用法。

##加速度计原理

![Accelerometer.png](http://images.90159.com/08/Accelerometer.png)

来自网络

iPhone的加速度计有三个轴，分别是x轴、y轴、z轴，如上图所示。这是一个三维立体空间，可以捕获用户每一个角度的操作。通过这三个轴可以计算iPhone倾斜的角度，从而计算出加速度。

##UIAccelerometer使用

`UIAccelerometer`的使用比较简单，需要实现`UIAccelerometerDelegate`的代理方法，具体代码如下：

    UIAccelerometer *accelerometer = [UIAccelerometer sharedAccelerometer];
    accelerometer.delegate = self;
    accelerometer.updateInterval = 0.1;
    
实现`UIAccelerometerDelegate`代理方法：

	- (void)accelerometer:(UIAccelerometer *)accelerometer didAccelerate:(UIAcceleration *)acceleration NS_DEPRECATED_IOS(2_0, 5_0)
	{
	    NSLog(@"x -> %f y - > %f z -> %f",accelerometer.x,accelerometer.y,accelerometer.z);
	}

##CoreMotion加速度计

首先需要引入`#import <CoreMotion/CoreMotion.h>`，直接通过`block`回调，然后处理三个轴的数据。代码如下：

	CMMotionManager *motionManager = [[CMMotionManager alloc] init];
    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
    //加速计
    if (motionManager.accelerometerAvailable) {
        motionManager.accelerometerUpdateInterval = 0.1;
        [motionManager startAccelerometerUpdatesToQueue:queue withHandler:^(CMAccelerometerData *accelerometerData,NSError *error){
            if (error) {
                [motionManager stopAccelerometerUpdates];
                NSLog(@"error");
            }else{
                NSLog(@"x -> %f y - > %f z -> %f",accelerometerData.acceleration.x,accelerometerData.acceleration.y,accelerometerData.acceleration.z);
            }
        }];
    }else{
        NSLog(@"This device has no accelerometer");
    }
    
以上就是关于`UIAccelerometer`和`CoreMotion`的使用，加速度计确实是iPhone手机的亮点功能。具体用途，需要我们发散思维，说不定哪一天你也有一个很牛的创意。屌丝逆袭也许就在下一刻。


