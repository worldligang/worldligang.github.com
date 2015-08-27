---
layout: post
title: 环形渐变进度条实现
date: 2015-08-12 01:12:05 +0800
comments: true
categories: 推荐
keywords: 进度条, 环形渐变, 切换动画, iOS开发, 刚刚在线
description: 之前有人在找渐变进度条的效果，闲来无事就顺手写了一个，然后画了视图层级，方便讲解。


---
之前有人在找渐变进度条的效果，闲来无事就顺手写了一个，然后画了视图层级，方便讲解。

环境信息：

Mac OS X 10.10.3

Xcode 6.3.1

iOS 8.3

效果图：


![1](http://images.90159.com/08/result.gif)

源码下载地址：[LoopProgressDemo](https://github.com/saitjr/LoopProgressDemo.git)

正文
##一、视图层级

首先需要搞定的就是视图层级关系。可以看到，

    1. 背景是有透明度的蓝色(blueView)

    2. 需要一个从绿->黄->红的渐变色，那个这里我采用的是Layer(colorLayer)

    3. blueView和colorLayer他们的表现状态都是环形的，所以还需要环形的遮罩

    4. 蓝色的环并没有变，而渐变色的环却在时刻变化，所以需要两个遮罩，一个给蓝色(blueMaskLayer)，一个给渐变色(colorMaskLayer)

从上面的分析可以看出，现在需要几个变量以及他们的关系如下：

	[viewController.view addSubView:blueView];
	[blueView.layer addSubLayer:colorLayer];
	colorLayer.mask = colorMaskLayer;
	blueView.layer.mask = blueMaskLayer;

<!--more-->
视图层级图如下：

![1](http://images.90159.com/08/ios-implement-loop-progress-1.png)

##二、根据视图层级来实现

搞清楚了层级，接着就应该逐个实现了。

1. 有透明度的蓝色视图blueView

ViewController.m

BlueView *blueView = [[BlueView alloc] initWithFrame:CGRectMake(0, 0, 300, 300)];
blueView.center = self.view.center;
blueView.backgroundColor = [UIColor blueColor]; // 我这里没有给透明度
[self.view addSubView:blueView];

2. 渐变图层colorLayer

因为系统没有提供根据路径渐变的实现方法，所以只能采用曲线救国的方式来画。那么我所使用的方法是：在左边画一个从下往上为绿->黄的渐变Layer（leftLayer），然后右边画一个从下往上为红->黄的渐变Layer（rightLayer）。

效果图如下：

![1](http://images.90159.com/08/ios-implement-loop-progress-2.png)

渐变色

 

但是这种方式效果不是很好，因为黄绿色和橘红色的分界太明显，所以最好设置一个渐变的范围。

效果图如下：

![1](http://images.90159.com/08/ios-implement-loop-progress-3.png)

设置渐变范围的渐变色

实现代码：

BlueView.m

	- (void)setupColorLayer {
	    self.colorLayer = [CAShapeLayer layer];
	    self.colorLayer.frame = self.bounds;
	    [self.layer addSublayer:self.colorLayer];
	
	    CAGradientLayer *leftLayer = [CAGradientLayer layer];
	    leftLayer.frame = CGRectMake(0, 0, self.bounds.size.width / 2, self.bounds.size.height);
	    // 分段设置渐变色
	    leftLayer.locations = @[@0.3, @0.9, @1];
	    leftLayer.colors = @[(id)[UIColor yellowColor].CGColor, (id)[UIColor greenColor].CGColor];
	    [self.colorLayer addSublayer:leftLayer];
	    
	    CAGradientLayer *rightLayer = [CAGradientLayer layer];
	    rightLayer.frame = CGRectMake(self.bounds.size.width / 2, 0, self.bounds.size.width / 2, self.bounds.size.height);
	    rightLayer.locations = @[@0.3, @0.9, @1];
	    rightLayer.colors = @[(id)[UIColor yellowColor].CGColor, (id)[UIColor redColor].CGColor];
	    [self.colorLayer addSublayer:rightLayer];
	}

3. 渐变图层的环形遮罩colorMaskLayer

因为渐变图层环形遮罩和蓝色视图的环形遮罩样式都是一样的，所以可以将环形遮罩的创建代码封装出来：

BlueView.m

	- (CAShapeLayer *)generateMaskLayer {
	    
	    CAShapeLayer *layer = [CAShapeLayer layer];
	    layer.frame = self.bounds;
	    
	    // 创建一个圆心为父视图中点的圆，半径为父视图宽的2/5，起始角度是从-240°到60°
	    UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:CGPointMake(self.bounds.size.width / 2, self.bounds.size.height / 2) radius:self.bounds.size.width / 2.5 startAngle:-3 / 4 * M_PI endAngle:1 / 3 * M_PI clockwise:YES];
	    layer.lineWidth = 20;
	    layer.path = path.CGPath;
	    layer.fillColor = [UIColor clearColor].CGColor; // 填充色为透明（不设置为黑色）
	    layer.strokeColor = [UIColor blackColor].CGColor; // 随便设置一个边框颜色
	    layer.lineCap = kCALineCapRound; // 设置线为圆角
	    return layer;
	}

设置渐变色环形遮罩层

BlueView.m

	- (void)setupColorMaskLayer {
	    
	    CAShapeLayer *layer = [self generateMaskLayer];
	    layer.lineWidth = 20.5; // 渐变遮罩线宽较大，防止蓝色遮罩有边露出来
	    self.colorLayer.mask = layer;
	    
	    self.colorMaskLayer = [CAShapeLayer layer];
	    self.colorMaskLayer = layer;
	}

4. blueView的环形遮罩blueMaskLayer

BlueView.m

	- (void)setupBlueMaskLayer {
	    
	    CAShapeLayer *layer = [self generateMaskLayer];
	    self.layer.mask = layer;
	    self.blueMaskLayer = layer;
	}

5. 设置百分比

设置渐变色所占的百分比，其实就是改变colorMaskLayer的范围，系统提供了一个方法可以直接根据百分比来修改。

	self.colorMaskLayer.strokeEnd = 0.5;

到此，整个效果就已经完成了，接下来说一下回弹动画。

##三、设置回弹动画

我所使用的是pop库中的POPSpringAnimation，这个效果比较Q弹，加在进度条上刚好。要加动画，只需要把上面的修改strokeEnd的代码换成一下方法就可以了：

	- (void)animationWithStrokeEnd:(CGFloat)strokeEnd {
	    
	    POPSpringAnimation *strokeAnimation = [POPSpringAnimation animationWithPropertyNamed:kPOPShapeLayerStrokeEnd];
	    strokeAnimation.toValue = @(strokeEnd);
	    strokeAnimation.springBounciness = 12.f;
	    strokeAnimation.removedOnCompletion = NO;
	    [self.colorMaskLayer pop_addAnimation:strokeAnimation forKey:@"layerStrokeAnimation"];
	}
	
来源：http://www.brighttj.com/ios/ios-implement-loop-progress.html