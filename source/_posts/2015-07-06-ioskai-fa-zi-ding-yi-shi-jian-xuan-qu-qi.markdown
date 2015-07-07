---
layout: post
title: "iOS开发自定义时间选取器"
date: 2015-07-06 23:40:21 +0800
comments: true
categories: iOS面试
keywords: iOS开发, 时间选取器, UIDatePicker, iOSDevTip, 刚刚在线
description: iOS开发自定义时间选取器背景是半透明的，点击的灰色背景的时候，时间选取器消失。在LGDatePickerView初始化方法里，代码如下

---


又是一年的暑假日期而至，小孩子放假，都会到在外打工的父母身边。孩子想父母，父母也思念着自己的孩子。我身边的亲戚朋友的孩子 也都来了。这个暑假又该热闹起来。

##努力什么时候都不晚

我有一个表妹，今年参加完高考，对自己的成绩不是特别满意。上次我回老家，刚好她给我一起来上海。准备来上海锻炼一下。

车上我问她，准备去哪上学？她说不想上了，想打工。我没有怎么劝她，我想让她体验一下打工的生活，她就知道还是上学好。

一个高中生，出来找工作，困难可想而知。经姐姐介绍，进了一家餐厅工作。工资也不是很高。干了两天，我问她怎么样？她说，高二要是来让我体验一下，我一定能考上一本。

也许真的如她所说的那样，但是谁知道呢？时间也不会倒流，她也不再可能再参加一次高考了。人只有吃过苦头，才知道努力。这是件好事，大学好好把握也是一样的。


**无论处于什么困境，只要知道努力，我觉得什么时候都不晚。**关注[iOS开发](http://www.superqq.com/)微信公众号：**iOSDevTip**

##自定义时间选取器

每天都随便扯扯，回到正题。今天我们做一个时间选取器，很简单，效果如下：

![time](http://7xjrlb.com1.z0.glb.clouddn.com/time.png)

我们自定义一个LGDatePickerView，在LGDatePickerView里面实现。

##背景半透明

背景是半透明的，点击的灰色背景的时候，时间选取器消失。在LGDatePickerView初始化方法里，代码如下：

	- (id)init
	{
	    self = [super init];
	    if (self) {
	  //背景半透明，绑定取消方法
	    UIControl *control = [[UIControl alloc] initWithFrame:SCREEN_BOUNDS];
	    control.backgroundColor = [UIColor colorWithRed:0.0/255.0 green:0.0/255.0 blue:0.0/255.0 alpha:0.5f];
	    [self addSubview:control];
	    [control addTarget:self action:@selector(actionCancel:) forControlEvents:UIControlEventTouchUpInside];	   
	     }
	    return self;
	}

	 
    
绑定的actionCancel方法：

	- (void)actionCancel:(id)sender
	{
	    [self removeFromSuperview];
	}

##确定取消按钮

看到上面的确定取消按钮，你会怎么做，写一个UIView上面放两个UIButton。这样做也是可以实现的。我们还可以用UIToolbar。在LGDatePickerView初始化方法里加上下面这段代码：

	 // Toolbar
    UIToolbar *toolbar = [[UIToolbar alloc] initWithFrame:CGRectMake(0, SCREEN.height - 250, SCREEN.width, 50)];
    toolbar.autoresizingMask = UIViewAutoresizingFlexibleTopMargin | UIViewAutoresizingFlexibleWidth;
    UIBarButtonItem *itemCancelDone = [[UIBarButtonItem alloc] initWithTitle:@"确定" style:UIBarButtonItemStylePlain target:self action:@selector(actionConfirm:)];
    UIBarButtonItem *itemCancel = [[UIBarButtonItem alloc] initWithTitle:@"取消" style:UIBarButtonItemStylePlain target:self action:@selector(actionCancel:)];
    UIBarButtonItem *space = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemFlexibleSpace target:nil action:nil];
    [toolbar setItems:[NSArray arrayWithObjects:itemCancel,space,itemCancelDone, nil]];
    [control addSubview:toolbar];
    
actionCancel上面已经实现了。下面实现actionConfirm方法。它有什么作用呢？

* 点击的时候获取到时间，然后通过代理代理出去
* 时间选取器消失

	
		- (void)actionConfirm:(id)sender
		{
		    if ([self.delegate respondsToSelector:@selector(datePickerView:didSelectTime:)]) {
		        [self.delegate datePickerView:self didSelectTime:self.datePicker.date];
		    }
		    [self removeFromSuperview];
		}


##代理方法

在LGDatePickerView.h

	@protocol LGDatePickerViewDelegate <NSObject>
	
	- (void)datePickerView:(LGDatePickerView *)datepicker didSelectTime:(NSDate *)time;
	
	@end

 
##创建UIDatePicker

在LGDatePickerView.h定义一个全局变量：

	@property (nonatomic, strong) UIDatePicker *datePicker;

在LGDatePickerView初始化方法里加上下面这段代码：

    UIDatePicker *datePicker = [[UIDatePicker alloc] init];
    datePicker.backgroundColor = [UIColor whiteColor];
    datePicker.datePickerMode = UIDatePickerModeCountDownTimer;
    datePicker.date = [NSDate date];
    datePicker.frame = CGRectMake(0, SCREEN.height - 200, SCREEN.width, 220);
    [control addSubview:datePicker];
    self.datePicker = datePicker;

##使用LGDatePickerView

使用起来很简单，创建一下，然后加载self.view上面即可：

		LGDatePickerView *datePicker = [[LGDatePickerView alloc] init];
	    datePicker.delegate = self;
	    datePicker.datePicker.date = [NSDate date];
	    datePicker.frame = self.view.bounds;
	    [self.view addSubview:datePicker];
    
以上就实现了iOS开发自定义时间选取器，代码不难，有什么问题，欢迎提问哈。
  