---
layout: post
title: UITablView上下滑动控制底部按钮的出现和消失
date: 2015-09-01 00:49:19 +0800
comments: true
categories: iOS开发
keywords: UITablView, 上下滑动, 按钮出现, iOS开发, 刚刚在线
description: 今天我们一起来做一个好玩的功能，通过UITablView上下滑动控制底部按钮的出现和消失。

---

今天我们一起来做一个好玩的功能，通过UITablView上下滑动控制底部按钮的出现和消失。先来看看效果：


##需求分析

要做一个这样的功能，对你来说应该不难，实现的方式也有很多。我们来分析一下要实现那几个小功能点：

* UITablView向上滑动，底部按钮消失
* UITablView向下滑动，底部按钮出现
* UITablView滑动到底部，底部按钮出现

主要就是这三个小功能点。那么很简单，我们只需要判断UITablView是往上滑动，还是往下滑动，以及判断UITablView是否滑动到底部即可。

##代码实现

###创建一个底部按钮

	self.bottomButton = [UIButton buttonWithType:UIButtonTypeCustom];
    self.bottomButton.frame = CGRectMake(SCREEN.width / 2 - 25, SCREEN.height - 50, 50, 50);
    [self.bottomButton setBackgroundImage:[UIImage imageNamed:@"bottom"] forState:UIControlStateNormal];
    [self.view addSubview:self.bottomButton];
    
既然是控制按钮的出现和消失，首先要创建一个底部按钮。SCREEN是个宏定义：

	#define SCREEN [UIScreen mainScreen].bounds.size

###判断UITablView上下滑动

我们都知道，UITablView是继承UIScrollView的。所以UIScrollView的代理方法在UITablView是可以使用的。要判断UITablView上下滑动，我们需要实现UIScrollView的scrollViewDidScroll:代理方法：

	-(void)scrollViewDidScroll:(UIScrollView *)scrollView{
	    if (scrollView.contentOffset.y > self.offsetY && scrollView.contentOffset.y > 0) {//向上滑动
	       
	    }else if (scrollView.contentOffset.y < self.offsetY ){//向上滑动
	       
	    }
	    self.offsetY = scrollView.contentOffset.y;//将当前位移变成缓存位移
	}
<!--more-->

###判断UITablView滑动到底部

也是在scrollViewDidScroll:这个方法里面判断：

	 //判断滑动到底部
    if (scrollView.contentOffset.y == scrollView.contentSize.height - self.tableView.frame.size.height) {
        
    }
###按钮消失和出现

	//按钮消失
	[UIView transitionWithView:self.bottomButton duration:0.1 options:UIViewAnimationOptionTransitionNone animations:^{
	            self.bottomButton.frame = CGRectMake(SCREEN.width / 2 - 25, SCREEN.height, 50, 50);
	        } completion:NULL];
	        
	//按钮出现        
    [UIView transitionWithView:self.bottomButton duration:0.1 options:UIViewAnimationOptionTransitionNone animations:^{
      self.bottomButton.frame = CGRectMake(SCREEN.width / 2 - 25, SCREEN.height - 50, 50, 50);
	  } completion:NULL];

按钮消失其实就是移除屏幕，这里我加了一个旋转的过度动画，需要什么动画效果都可以选择。按钮出现就是把按钮的frame设置到初始化时候的frame即可。

这样我们就实现了通过UITablView上下滑动控制底部按钮的出现和消失。代码我已经上传的github，下载地址还是[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)

如果你觉得文章还不错，请分享给你的同学和朋友，欢迎推荐他们关注iOS开发：iOSDevTip 公众号。