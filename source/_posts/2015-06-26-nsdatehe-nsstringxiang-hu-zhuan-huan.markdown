---
layout: post
title: "NSDate和NSString相互转换"
date: 2015-06-26 07:34:32 +0800
comments: true
categories: iOS开发
keywords: NSString转NSDate, NSDate, NSString, NSDate NSString,  NSDate转NSString, 刚刚在线
description: NSString转NSDate,不积小流无以成江海，不及硅步无以至千里。NSDate转NSString，做iOS开发也是这样，平时写代码要多收集一些有用的代码，以免每次都从新来一遍，不仅浪费时间，也影响开发效率。

---
![1](http://7xjrlb.com1.z0.glb.clouddn.com/2NSDate.jpg)

不积小流无以成江海，不及硅步无以至千里。做[iOS开发](http://www.superqq.com/blog/2015/01/14/ioskai-fa-zhi-shou-shi-shi-bie-hui-zong/)也是这样，平时写代码要多收集一些有用的代码，以免每次都从新来一遍，不仅浪费时间，也影响开发效率。

有人把做app形容成盖房子，码农就是搬砖的，真的是形象无比。随着时间的推移，手里积累的优秀代码就越来越多。这样，盖起房子来，效率肯定是杠杠的。

我也会收集一些有用的代码，有的会写在博客里，有的会写成demo放在[github](http://www.superqq.com/blog/2014/11/19/ioskai-fa-zhong-gitfen-zhi-chuang-jian-he-guan-li/)上面，有的就很安静的躺在我的电脑里。每次我用到的时候，我都会顺手复制过来。

今天给大家分享一些关于NSDate和NSString相互转换。百度一下，有一大推相关的方法。每次都百度，不如自己收集起来方便。

##NSDate转NSString

日期转成字符串。这个虽然简单，但是我相信很多朋友初次遇到肯定束手无策。脑子里蹦出四个字：这怎么转？直接上代码：

	//获取系统当前时间
	NSDate *currentDate = [NSDate date];
	//用于格式化NSDate对象
    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
    //设置格式：zzz表示时区
    [dateFormatter setDateFormat:@"yyyy-MM-dd HH:mm:ss zzz"];
    //NSDate转NSString
    NSString *currentDateString = [dateFormatter stringFromDate:currentDate];
    //输出currentDateString
    NSLog(@"%@",currentDateString);


NSDate对象包含两个部分，日期（Date）和时间（Time）。格式化的时间字符串主要也是针对日期和时间的。NSDateFormatter是一个很常用的类，用于格式化NSDate对象，支持本地化的信息。

NSDateFormatter常用的格式有：

	yyyy-MM-dd HH:mm:ss.SSS 
	yyyy-MM-dd HH:mm:ss
	yyyy-MM-dd
	MM dd yyyy

NSDateFormatter格式化参数如下：

    G: 公元时代，例如AD公元
    yy: 年的后2位
    yyyy: 完整年
    MM: 月，显示为1-12
    MMM: 月，显示为英文月份简写,如 Jan
    MMMM: 月，显示为英文月份全称，如 Janualy
    dd: 日，2位数表示，如02
    d: 日，1-2位显示，如 2
    EEE: 简写星期几，如Sun
    EEEE: 全写星期几，如Sunday
    aa: 上下午，AM/PM
    H: 时，24小时制，0-23
    K：时，12小时制，0-11
    m: 分，1-2位
    mm: 分，2位
    s: 秒，1-2位
    ss: 秒，2位
    S: 毫秒

##NSString转NSDate

既然NSDate可以转成NSString，毫无疑问NSString也可以转成NSDate。代码如下：

	//需要转换的字符串
	NSString *dateString = @"2015-06-26 08:08:08";
	 //设置转换格式
    NSDateFormatter *formatter = [[NSDateFormatter alloc] init] ;
    [formatter setDateFormat:@"yyyy-MM-dd HH:mm:ss"];
    //NSString转NSDate
    NSDate *date=[formatter dateFromString:dateString];

NSDate和NSString相互转换就是这么简单。

##转换工具类

在项目中，我们需要用到转换的地方可能不止一处，所以建议我们定义一个工具类。在工具类里实现如下两个方法：
	
	//NSDate转NSString
	+ (NSString *)stringFromDate:(NSDate *)date
	{
	    //获取系统当前时间
	    NSDate *currentDate = [NSDate date];
	    //用于格式化NSDate对象
	    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
	    //设置格式：zzz表示时区
	    [dateFormatter setDateFormat:@"yyyy-MM-dd HH:mm:ss zzz"];
	    //NSDate转NSString
	    NSString *currentDateString = [dateFormatter stringFromDate:currentDate];
	    //输出currentDateString
	    NSLog(@"%@",currentDateString);
	    return currentDateString;
	}
	
	//NSString转NSDate
	+ (NSDate *)dateFromString:(NSString *)string
	{
	    //需要转换的字符串
	    NSString *dateString = @"2015-06-26 08:08:08";
	    //设置转换格式
	    NSDateFormatter *formatter = [[NSDateFormatter alloc] init] ;
	    [formatter setDateFormat:@"yyyy-MM-dd HH:mm:ss"];
	    //NSString转NSDate
	    NSDate *date=[formatter dateFromString:dateString];
	    return date;
	}

有了这个工具类，妈妈再也不用担心我的转换能力啦！

>作者李刚是刚刚在线（[www.superqq.com](www.superqq.com)）站长，百度百家专栏作者
>iOS工程师非著名自媒体，微信公众号iOS开发：iOSDevTip运营者

![2](http://7xjrlb.com1.z0.glb.clouddn.com/ios.png)