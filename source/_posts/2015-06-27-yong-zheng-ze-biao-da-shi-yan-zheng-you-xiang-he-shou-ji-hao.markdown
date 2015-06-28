---
layout: post
title: "用正则表达式验证邮箱和手机号"
date: 2015-06-27 07:35:42 +0800
comments: true
categories: iOS开发
keywords: 正则表达式, iOS正则表达式, 邮箱正则表达式,  手机号正则表达式, 刚刚在线
description: 正则表达式，又称正规表示法、常规表示法（英语：Regular Expression，在代码中常简写为regex、regexp或RE），计算机科学的一个概念。正则表达式使用单个字符串来描述、匹配一系列符合某个句法规则的字符串。


---

>正则表达式，又称正规表示法、常规表示法（英语：Regular Expression，在代码中常简写为regex、regexp或RE），计算机科学的一个概念。正则表达式使用单个字符串来描述、匹配一系列符合某个句法规则的字符串。


做iOS开发的童鞋，应该都用过正则表达式吧。正则表达式很好地，帮助我们判断一个字符串是否合法。比如：

1. 在做app注册页面的时候，需要判断手机号是否格式正确，是否够11位。

2. 在做意见反馈的时候，需要判断邮箱格式是否正确。

判断手机号和判断邮箱，应该是iOS开发者们最常用的。具体如何判断，简单整理如下：

##验证邮箱

	+ (BOOL)validateEmail:(NSString *)email
	{
	    NSString *emailRegex = @"[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,4}";
	    NSPredicate *emailTest = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", emailRegex];
	    return [emailTest evaluateWithObject:email];
	}

NSPredicate是一个Foundation类，是用来查询的，原理和用法都类似于SQL中的where。


##验证手机号

###简单的判断方法

	+ (BOOL)validatePhone:(NSString *)phone
	{
	    NSString *phoneRegex = @"1[3|5|7|8|][0-9]{9}";
	    NSPredicate *phoneTest = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", phoneRegex];
	    return [phoneTest evaluateWithObject:phone];
	}
	
这只是简单地判断手机号格式。其实手机的格式还是有一点复杂的。

###详细的判断方法

	//正则判断手机号码格式
	+ (BOOL)validatePhone:(NSString *)phone
	{
	       /**
	        * 手机号码
	        * 移动：134[0-8],135,136,137,138,139,150,151,157,158,159,182,187,188
	        * 联通：130,131,132,152,155,156,185,186
	        * 电信：133,1349,153,180,189
	        */
	       NSString * MOBILE = @"^1(3[0-9]|5[0-35-9]|8[025-9])\\d{8}$";
	       /**
	        10         * 中国移动：China Mobile
	        11         * 134[0-8],135,136,137,138,139,150,151,157,158,159,182,187,188
	        12         */
	       NSString * CM = @"^1(34[0-8]|(3[5-9]|5[017-9]|8[278])\\d)\\d{7}$";
	       /**
	        15         * 中国联通：China Unicom
	        16         * 130,131,132,152,155,156,185,186
	        17         */
	       NSString * CU = @"^1(3[0-2]|5[256]|8[56])\\d{8}$";
	       /**
	        20         * 中国电信：China Telecom
	        21         * 133,1349,153,180,189
	        22         */
	       NSString * CT = @"^1((33|53|8[09])[0-9]|349)\\d{7}$";
	       /**
	        25         * 大陆地区固话及小灵通
	        26         * 区号：010,020,021,022,023,024,025,027,028,029
	        27         * 号码：七位或八位
	        28         */
	      // NSString * PHS = @"^0(10|2[0-5789]|\\d{3})\\d{7,8}$";
	    
	     NSPredicate *regextestmobile = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", MOBILE];
	     NSPredicate *regextestcm = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", CM];
	     NSPredicate *regextestcu = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", CU];
	     NSPredicate *regextestct = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", CT];
	    
	    if (([regextestmobile evaluateWithObject:phone] == YES)
	    || ([regextestcm evaluateWithObject:phone] == YES)
	    || ([regextestct evaluateWithObject:phone] == YES)
	    || ([regextestcu evaluateWithObject:phone] == YES))
	    {
	        if([regextestcm evaluateWithObject:phone] == YES) {
	          NSLog(@"China Mobile");
	        } else if([regextestct evaluateWithObject:phone] == YES) {
	          NSLog(@"China Telecom");
	        } else if ([regextestcu evaluateWithObject:phone] == YES) {
	          NSLog(@"China Unicom");
	        } else {
	          NSLog(@"Unknow");
	        }
	        
	        return YES;
	    }
	    else 
	    {
	        return NO;
	    }
	}


以上这段（来自网络）是手机号判断的详细方法。基本上这个判断就够用了，如果三大运营商，再出其他段的手机号。直接子啊上面做简单修改即可。

>作者李刚是刚刚在线（[www.superqq.com](www.superqq.com)）站长，百度百家专栏作者
>iOS工程师非著名自媒体，微信公众号iOS开发：iOSDevTip运营者

![2](http://7xjrlb.com1.z0.glb.clouddn.com/ios.png)