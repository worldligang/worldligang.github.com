---
layout: post
title: "UIButton文字居左显示"
date: 2015-07-02 22:18:01 +0800
comments: true
categories: iOS开发
keywords: UIButton, UIButton文字居左, contentHorizontalAlignment, UIButton居左, 刚刚在线
description: 今天我们来讲如何让UIButton文字居左显示？我们都应该写过让UILabel的text居左显示。代码也非常简单。

---

题外话：时间依旧过的很快，不知不觉2015年就过去一半了。感觉自己好像没有大的改变，只能感叹时间飞逝，却不能有所收获。

我从来都不是一个安于现状的人，改变自己的想法从未停止过。我想大多数人都跟我有类似的想法。但是为什么很难有所成就呢？我觉得最重要的原因就是，**只是想一下，而没有去行动**。

这是一个全民创业的时代，不把握机会，只能错过。错过的机会，越多遗憾就越多。有句话说：**老了之后，回想人生，不会因为自己做过什么而后悔，而会因为自己没有做过什么而遗憾。**不想给人生留下遗憾，所以努力去尝试是唯一的选择。

随便感慨一下。每天上班太舒服了，淡忘了自己的理想，借此提醒一下自己。

来来，收回思路，今天我们来讲如何让UIButton文字居左显示？我们都应该写过让UILabel的text居左显示。代码也非常简单。

##UILabel文字居左显示

实现文字居左显示代码如下：

	UILabel *label = [[UILabel alloc] init];
    label.frame = CGRectMake(50, 100, 200, 50);
    label.text = @"我是label";
    label.textAlignment = NSTextAlignmentLeft;
    label.backgroundColor = [UIColor orangeColor];
    [self.view addSubview:label];
    
运行起来看一下效果：

![1](http://7xjrlb.com1.z0.glb.clouddn.com/labelleft.png)    

label.backgroundColor = [UIColor orangeColor];设置label的背景颜色方便我们参考。你是不是这样做的。依次类推UIButton也很简单，你肯定能想到。我们来看看。

##UIButton文字居左显示

###创建UIButton

	UIButton *button = [[UIButton alloc] init];
    //设置坐标
    button.frame = CGRectMake(100, 100, 100, 50);
    //设置标题
    [button setTitle:@"我是UIButton" forState:UIControlStateNormal];
    //设置标题颜色
    [button setTitleColor:[UIColor blueColor] forState:UIControlStateNormal];
    //设置背景颜色    
    [button setBackgroundColor:[UIColor orangeColor]];
    [self.view addSubview:button];

以上代码是创建一个button，设置坐标、标题、和标题颜色。

###让文字居左

按照UILabel文字居左的写法，UIButton应该这么写：

	 button.titleLabel.textAlignment = NSTextAlignmentLeft;

运行一下看一下效果：

![2](http://7xjrlb.com1.z0.glb.clouddn.com/buttonnoleft.png)

我们发现UIButton的文字还是居中显示。竟然没有居左显示，怎么办呢？进UIButton看看，还有哪些属性。很快发现：

	@property(nonatomic) UIControlContentHorizontalAlignment contentHorizontalAlignment; // how to position content hozontally inside control. default is center
	
	typedef NS_ENUM(NSInteger, UIControlContentHorizontalAlignment) {
	    UIControlContentHorizontalAlignmentCenter = 0,
	    UIControlContentHorizontalAlignmentLeft   = 1,
	    UIControlContentHorizontalAlignmentRight  = 2,
	    UIControlContentHorizontalAlignmentFill   = 3,
	};
	
###设置contentHorizontalAlignment

	button.contentHorizontalAlignment = UIControlContentHorizontalAlignmentLeft;

再次运行起来看效果：

![3](http://7xjrlb.com1.z0.glb.clouddn.com/buttonleft.png)
	
果然可以，是不是看着居左显示很难看，太靠边了。很简单，设置UIButton的titleEdgeInsets属性：
	
	button.titleEdgeInsets = UIEdgeInsetsMake(0, 10, 0, 0);

这样button的title就距左边10个像素的距离。

居右显示就很简单了：

	button.contentHorizontalAlignment = UIControlContentHorizontalAlignmentRight;

ok，UIButton文字居左显示完成了。更多iOS技术请点击[刚刚在线](www.superqq.com) : **www.superqq.com**