---
layout: post
title: "CGContextRef使用简要教程"
date: 2015-06-29 10:55:01 +0800
comments: true
categories: iOS开发
keywords: CGContextRef, CGContextRef使用简要教程, CGContextRef教程, iOS CGContextRef, 刚刚在线
description: CGContextRef功能强大，我们借助它可以画各种图形。开发过程中灵活运用这些技巧，可以帮助我们提供代码水平。

---
Graphics Context是图形上下文,也可以理解为一块画布,我们可以在上面进行绘画操作,绘制完成后,将画布放到我们的view中显示即可,view看作是一个画框.

CGContextRef功能强大，我们借助它可以画各种图形。开发过程中灵活运用这些技巧，可以帮助我们提供代码水平。

说到画图，我就立马想到：我的数学公式都快忘完了。


##高中数学你还记得多少？

我记得我上学的时候，数学一直都很好。以至于在班里都不敢和别人吵吵闹闹。一吵吵闹闹，别人就会来一句：以为你数学好，就可以怎么样怎么样了是吧？说的我好无奈啊。

这么多年过去了，说句实话，高中时学的数学公式什么的，都忘记的差不多了。现在要去做高三的考题，估计比写代码还难。

作为一名程序员，数学好，一定是优势。就拿画图来说吧，如果你还清楚的记得那些公式，那么你就可以很随意地画出一些好看的图形出来。

接下来，我们来看看CGContextRef到底能实现些什么？

##写文字

   
    
    - (void)drawRect:(CGRect)rect
	{
	    //获得当前画板
	    CGContextRef ctx = UIGraphicsGetCurrentContext();
	    //颜色
	    CGContextSetRGBStrokeColor(ctx, 0.2, 0.2, 0.2, 1.0);
	    //画线的宽度
	    CGContextSetLineWidth(ctx, 0.25);
	    //开始写字
	    [@"我是文字" drawInRect:CGRectMake(10, 10, 100, 30) withFont:font];  
	    [super drawRect:rect];
	}

这段代码就可以很漂亮的写出四个大字：我是文字。很容易理解，每句话都有注释。


##画直线

	- (void)drawRect:(CGRect)rect
	{
	    //获得当前画板
	    CGContextRef ctx = UIGraphicsGetCurrentContext();
	    //颜色
	    CGContextSetRGBStrokeColor(ctx, 0.2, 0.2, 0.2, 1.0);
	    //画线的宽度
	    CGContextSetLineWidth(ctx, 0.25);
	    //顶部横线
	    CGContextMoveToPoint(ctx, 0, 10);
	    CGContextAddLineToPoint(ctx, self.bounds.size.width, 10);
	    CGContextStrokePath(ctx);
	    [super drawRect:rect];
	}

##画圆

    - (void)drawRect:(CGRect)rect
	{
	    //获得当前画板
	    CGContextRef ctx = UIGraphicsGetCurrentContext();
	    //颜色
	    CGContextSetRGBStrokeColor(ctx, 0.2, 0.2, 0.2, 1.0);
	    //画线的宽度
	    CGContextSetLineWidth(ctx, 0.25);
	    //void CGContextAddArc(CGContextRef c,CGFloat x, CGFloat y,CGFloat radius,CGFloat startAngle,CGFloat endAngle, int clockwise)1弧度＝180°/π （≈57.3°） 度＝弧度×180°/π 360°＝360×π/180 ＝2π 弧度
	    // x,y为圆点坐标，radius半径，startAngle为开始的弧度，endAngle为 结束的弧度，clockwise 0为顺时针，1为逆时针。
    	CGContextAddArc(ctx, 100, 20, 20, 0, 2*M_PI, 0); //添加一个圆
	    CGContextDrawPath(ctx, kCGPathStroke); //绘制路径
	    [super drawRect:rect];
	}
	
这个画圆的公式你还记得吗？你还知道M_PI是什么吗？等于多少吗？赶紧脑补一下吧！

##画矩形

	- (void)drawRect:(CGRect)rect
	{
	    //获得当前画板
	    CGContextRef ctx = UIGraphicsGetCurrentContext();
	    //颜色
	    CGContextSetRGBStrokeColor(ctx, 0.2, 0.2, 0.2, 1.0);
	    //画线的宽度
	    CGContextSetLineWidth(ctx, 0.25);
	    CGContextAddRect(ctx, CGRectMake(2, 2, 30, 30));
	    CGContextStrokePath(ctx);
	    [super drawRect:rect];
	}

图形千千万，万变不离其宗。更多画图方法，期待你的探索。


>作者李刚是刚刚在线（[www.superqq.com](www.superqq.com)）站长，百度百家专栏作者
>iOS工程师非著名自媒体，微信公众号iOS开发：iOSDevTip运营者

![2](http://7xjrlb.com1.z0.glb.clouddn.com/ios.png)
