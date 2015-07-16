---
layout: post
title: "NSString的八条实用技巧"
date: 2015-07-16 23:07:57 +0800
comments: true
categories: iOS开发
keywords: NSString, NSString实用技巧, 实用技巧, 技巧, 刚刚在线
description:  今天这篇，我们讲讲NSString的八条实用技巧。大家可以收藏起来，方便开发随时可以复制粘贴。
 
---

有一篇文章写了：[iOS开发之NSString的几条实用技巧](http://www.superqq.com/blog/2015/07/11/ioskai-fa-zhi-nsstringde-ji-tiao-shi-yong-ji-qiao/) ， 今天这篇，我们讲讲NSString的八条实用技巧。大家可以收藏起来，方便开发随时可以复制粘贴。

##0.首字母大写

代码：

	//首字母大写
    NSString *string = @"ligang";
    NSLog(@"string: %@",[string capitalizedString]);
   
打印：
   
    2015-07-16 23:06:11.652 iOSStrongDemo[10279:3062010] string: Ligang

##1.分割字符串

代码：

	//分割字符串
    NSString *string = @"This is a iOSDevTip";
    NSArray *array = [string componentsSeparatedByString:@"a"];
    NSString *string1 = [array objectAtIndex:0];
    NSString *string2 = [array objectAtIndex:1];
    NSLog(@"string1:%@  string2:%@",string1,string2);
 
打印：  
    
	2015-07-16 22:40:39.559 iOSStrongDemo[10165:3055448] string1:This is   string2: iOSDevTip

##2.追加字符串

代码：

    //追加字符串
    NSMutableString *string = [[NSMutableString alloc] initWithString:@"I Love "];
    [string appendString:@"China"];
    NSLog(@"string:%@",string);
   
打印：    
    
    2015-07-16 22:42:32.305 iOSStrongDemo[10189:3056410] string:I Love China

##3.插入字符串

代码：

	//插入字符串
    NSMutableString *string = [[NSMutableString alloc] initWithString:@"I China"];
    [string insertString:@"Love " atIndex:2];
    NSLog(@"string: %@",string);
    
打印：    
    
    2015-07-16 22:44:10.706 iOSStrongDemo[10206:3057014] string: I Love China

##4.删除字符串

代码：
 	
 	//删除字符串
	NSMutableString *string = [[NSMutableString alloc] initWithString:@"I love China"];
    [string deleteCharactersInRange:NSMakeRange(2, 4)];
    NSLog(@"String1: %@",string);

打印：

	2015-07-16 22:46:58.437 iOSStrongDemo[10219:3057749] String1: I  China

##5.判断是否包含前后缀

代码：

 	//判断是否包含前后缀
    NSString *string = @"I love China";
    BOOL isHasI = [string hasPrefix:@"I"];
    BOOL isHasChina = [string hasSuffix:@"China"];
    
##6.替换字符串

代码：

	//替换字符串
    NSString *string = @"I love China";
    NSString *replaceString = [string stringByReplacingOccurrencesOfString:@"love" withString:@"like"];
    NSLog(@"replaceString:  %@",replaceString);
    
打印：    
    
    2015-07-16 22:56:07.405 iOSStrongDemo[10236:3059503] replaceString:  I like China

##7.去除字符串首尾的空格和换行符

代码：

	//去除字符串首尾的空格和换行符
    NSString *string = @" I love China ";
    NSString *text = [string stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]];
    NSLog(@"text:%@",text);
    
    
打印：
    
    2015-07-16 23:00:47.845 iOSStrongDemo[10265:3061013] text:I love China

代码已经更新，demo下载地址：[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)
