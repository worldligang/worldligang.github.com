---
layout: post
title: 彩色TabBar切换动画实现
date: 2015-08-11 00:38:31 +0800
comments: true
categories: 推荐
keywords: TabBar, 动画, 切换动画, iOS开发, 刚刚在线
description: 看到这个彩色切换效果的时候，我第一个反应就是在TabBar上有一个彩色的View，然后每个色块的显示都是通过mask来显示的。最终，我的具体实现也是根据这个思路来的。

---



无意间看到一个彩色TabBar切换的设计图，感觉很不错，有空就把他实现了。
环境信息
Mac OS X 10.10.4
Xcode 6.4
iOS 8.4
效果图：


![1.1TabBar-1.gif](http://images.90159.com/08/1.1TabBar-1.gif)

源码下载地址：
[https://github.com/saitjr/TColorfulTabBar.git](https://github.com/saitjr/TColorfulTabBar.git)


##一、实现分析


看到这个彩色切换效果的时候，我第一个反应就是在TabBar上有一个彩色的View，然后每个色块的显示都是通过mask来显示的。最终，我的具体实现也是根据这个思路来的。

####1. 设计思想
为了减少侵入性（耦合），我采取的是继承UITabBar来实现，所以要集成的时候，只需要将系统的TabBar换成我写的TColorfulTabBar就可以了。

![1.2TabBar-2.png](http://images.90159.com/08/1.2TabBar-2.png)

####2. 视图层级图

##二、效果实现

####1. 添加彩色视图colorfulView

TColorfulTabBar.m
	
	- (void)setupColorView {
	    
	    // 初始化彩色视图，并将它加在tabbar上
	    UIView *colorView = [[UIView alloc] initWithFrame:self.bounds];
	    [self addSubview:colorView];
	    self.colorfulView = colorView;
	    
	    // 彩色视图的五种颜色，这是一个UIColor数组，
	    NSArray *colors = self.itemColors;
	    CGFloat itemWidth = self.bounds.size.width / self.itemCount;
	    
	    for (int i = 0; i < self.itemCount; i ++) {
	        
	        UIView *view = [[UIView alloc] initWithFrame:CGRectMake(itemWidth * i, 0, itemWidth, self.bounds.size.height)]; 
	        view.backgroundColor = colors[i]; 
	        [self.colorfulView addSubview:view]; 
	    } 
	}
	
<!--more-->

####2. 添加彩色视图的遮罩colorfulMaskView

之所以这里的遮罩使用的是UIView而不是CAShapeLayer或者CALayer，原因如下：

* 遮罩是矩形，没必要使用路径，所以不用CAShapeLayer
* 通过设计图可以看到，在位移动画之前，有一个遮罩单向变宽（向左或向右）的效果。如果使用CALayer，那么改变bounds的时候，是双向改变，想要单向就必须加锚点，太麻烦
* 如果使用UIView，那么向右变宽只需要x不变，width变长就行；向左变宽只需要x-value，width+value就行，要方便的多
* 最后一点就是在做动画的时候，Layer要用到CABaseAnimation，然而UIView使用UIView的Animation就行

在设置遮罩的时候，将colorfulMaskView的layer设置为colorfulView的mask就可以了。
代码实现如下：
TColorfulTabBar.m

	- (void)setupMaskLayer {
	    
	    // 获取每个item的宽度
	    CGFloat itemWidth = self.bounds.size.width / self.itemCount;
	    
	    // 初始化colorMaskView，并将colorMaskView的layer设置成彩色视图的遮罩
	    UIView *colorMaskView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, itemWidth, self.bounds.size.height)];
	    colorMaskView.backgroundColor = [UIColor blackColor];
	    self.colorfulMaskView = colorMaskView;
	    self.colorfulView.layer.mask = self.colorfulMaskView.layer;
	}
	
####3. 设置代理，获取当前点击的下标
UITabBar并没有获取点击下标的接口，但是UITabBarDelegate有，这个协议默认被UITabBarController遵守。为了降低耦合性，采用UITabBar自己获取下标，所以，需要UITabBar自己实现自己的delegate。
**值得注意的是，不能在UITabBar的初始化方法中去设置self.delegate = self，因为即使设置了，也会被UITabBarController覆盖。最终解决方案是当UITabBar加载到父视图上是，在修改delegate为self。**

	- (void)didMoveToSuperview {
	    [super didMoveToSuperview];
	    self.delegate = self;
	}
因为每次的移动位置与方向都和上次的下标与这次的下标有关，所以需要使用到属性来记录这两个下标的值。
	- (void)tabBar:(UITabBar *)tabBar didSelectItem:(UITabBarItem *)item {
	    
	    NSInteger index = [self.items indexOfObject:item];
	    self.fromeIndex = self.toIndex;
	    self.toIndex = index;
	    // 拿到下标以后执行动画
	    [self animation];
	}
####4. 移动遮罩，显示不同的色块

在做动画的时候，需要考虑到效果的平滑性。整个动画有两个动画组成，一个是宽度放大的动画，一个是缩小到原来大小和位移动画。整个动画是EaseInOut的效果，所以拆开来看，就应该第一个动画EaseIn，第二个动画EaseOut。
一图胜千言：

![1.3TabBar-3.png](http://images.90159.com/08/1.3TabBar-3.png)

	- (void)animation {
	    
	    CGFloat itemWidth = self.bounds.size.width / self.itemCount;
	    // 为了效果看起来更平滑，所以根据两次下标的差值来计算需要放大的距离
	    CGFloat extraWidth = ABS(self.toIndex - self.fromeIndex) * itemWidth / 4;
	    
	    // 放大的大小
	    CGRect scaleFrame = CGRectMake(self.colorfulMaskView.x, 0, itemWidth + extraWidth, self.bounds.size.height);
	    // 最终大的大小与位置
	    CGRect toFrame = CGRectMake(self.toIndex * itemWidth, 0, itemWidth, self.bounds.size.height);
	    
	    // 如果是向左移动，那修改x轴的坐标
	    if (self.fromeIndex > self.toIndex) {
	        
	        scaleFrame = CGRectMake(self.colorfulMaskView.x - extraWidth, 0, itemWidth + extraWidth, self.bounds.size.height);
	    }
	    
	    // 两个动画加起来是一个EaseInOut的效果，所以第一个动画就应该是EaseIn，第二个动画是EaseOut
	    // 先进行放大
	    [UIView animateWithDuration:ANIMATION_DURATION delay:0 options:UIViewAnimationOptionCurveEaseIn animations:^{
	        
	        self.colorfulMaskView.frame = scaleFrame;
	    } completion:^(BOOL finished) {
	        
	        // 再进行缩小和位移
	        [UIView animateWithDuration:ANIMATION_DURATION delay:0 options:UIViewAnimationOptionCurveEaseOut animations:^{
	            
	            self.colorfulMaskView.frame = toFrame;
	        } completion:NULL];
	    }];
	}
	
原文:http://www.brighttj.com/ios/ios-colorful-tabbar-implement.html