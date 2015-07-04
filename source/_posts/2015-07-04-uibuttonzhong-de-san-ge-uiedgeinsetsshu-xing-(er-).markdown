---
layout: post
title: "UIButton中的三个UIEdgeInsets属性（二）"
date: 2015-07-04 14:14:43 +0800
comments: true
categories: iOS开发
keywords: UIEdgeInsets, UIEdgeInsets, contentEdgeInsets, titleEdgeInsets, imageEdgeInsets, 刚刚在线
description: 接着昨天的UIButton中的三个UIEdgeInsets属性，今天我们具体谈谈UIButton的contentEdgeInsets、titleEdgeInsets、imageEdgeInsets属性。

---

昨天去公司加班的人请举手，不是只有我一个吧？（😭）我觉得加班的人应该不少吧。今天周日，应该不需要去加班了。心里暗暗自喜，终于可以休息一天啦。（😄）

说到加班，应该是程序猿心里的痛啊。不加班吧，东西做不完；加班吧，身体吃不消。作为程序猿真的不容易，每天从早忙到晚。突然有一天，六点下班，看见太阳还在等我，心里被踢有多高兴。

弱弱的问一下：加班好不好？

要看这个问题问谁了。如果问老板，老板肯定美滋滋的说，加班好啊，说明你有责任心，有上进心，把公司当成自己的...

如果问我，哈哈，我当然说加班不好啦，不然会被打死的。开个玩笑。说句实话，加班肯定是不好的，尤其是对程序猿来说。每天面对电脑，每天坐着，时间长了，各种问题就凸显出来。

但是，我说加班不好有用吗？如果我要是老板，我就直接让员工在家办公。（😊）所以大家感觉支持我吧，说不定哪天我真的...

接着昨天的[UIButton中的三个UIEdgeInsets属性](http://www.superqq.com/blog/2015/07/03/uibuttonzhong-de-san-ge-uiedgeinsetsshu-xing-%28%5B%3F%5D-%29/)，今天我们具体谈谈UIButton的contentEdgeInsets、titleEdgeInsets、imageEdgeInsets属性。

##创建UIButton

    UIButton *button = [[UIButton alloc] init];
    button.frame = CGRectMake(50, 200, 200, 50);
    [button setTitle:@"我是UIButton" forState:UIControlStateNormal];
    [button setTitleColor:[UIColor blueColor] forState:UIControlStateNormal];
    [button setBackgroundColor:[UIColor orangeColor]];
    button.titleLabel.textAlignment = NSTextAlignmentLeft;
    button.contentHorizontalAlignment = UIControlContentHorizontalAlignmentLeft;
    [self.view addSubview:button];

创建一个button，让button的title居左，以便观察：

![1](http://7xjrlb.com1.z0.glb.clouddn.com/button1.png)


##UIButton的contentEdgeInsets属性

	@property(nonatomic)          UIEdgeInsets contentEdgeInsets UI_APPEARANCE_SELECTOR; // default is UIEdgeInsetsZero

contentEdgeInsets里有一个content应该指的就是UIButton的title。

###参数含义

上一篇文章我们讲了UIEdgeInsets是个结构体类型。里面有四个参数，分别是：top, left, bottom, right。这四个参数表示距离上边界、左边界、下边界、右边界的距离。

这四个参数的值可以为正值，也可以为负值。拿left举例：

	left = 10; //代表以当前位置为基准，向右移动10个像素
	left = -10; //代表以当前位置为基准，向左移动10个像素

###向右移动20个像素

	button.contentEdgeInsets = UIEdgeInsetsMake(0, 20, 0, 0);

向右移动20个像素，left = 20，就可以了。

![2](http://7xjrlb.com1.z0.glb.clouddn.com/button2.png)


###向左移动20个像素

	button.contentEdgeInsets = UIEdgeInsetsMake(0, -20, 0, 0);

![2](http://7xjrlb.com1.z0.glb.clouddn.com/button3.png)


##UIButton的titleEdgeInsets属性

titleEdgeInsets和contentEdgeInsets的作用差不多。我们及设置contentEdgeInsets，又设置titleEdgeInsets，会怎样呢？

	button.titleEdgeInsets = UIEdgeInsetsMake(0, 20, 0, 0);
    button.contentEdgeInsets = UIEdgeInsetsMake(0, 20 , 0, 0);

看一下效果：

![3](http://7xjrlb.com1.z0.glb.clouddn.com/button4.png)

##UIButton的titleEdgeInsets属性

###创建一个带照片的button

	UIButton *button = [[UIButton alloc] init];
    button.frame = CGRectMake(50, 200, 200, 200);
    [button setTitleColor:[UIColor blueColor] forState:UIControlStateNormal];
    [button setBackgroundColor:[UIColor orangeColor]];
    [button setImage:[UIImage imageNamed:@"test"] forState:UIControlStateNormal];
    [self.view addSubview:button];

运行一下：

![5](http://7xjrlb.com1.z0.glb.clouddn.com/button5.png)

###向右移动50个像素

	button.imageEdgeInsets = UIEdgeInsetsMake(0, 50, 0, 0);

看看效果：

![6](http://7xjrlb.com1.z0.glb.clouddn.com/button6.png)

	
###向左移动50个像素

	button.imageEdgeInsets = UIEdgeInsetsMake(0, -50, 0, 0);

看看效果：

![7](http://7xjrlb.com1.z0.glb.clouddn.com/button7.png)

大家可以自行设置其他三个参数看看效果是怎样的，自己动手便于理解。

![ios](http://7xjrlb.com1.z0.glb.clouddn.com/ios.png)