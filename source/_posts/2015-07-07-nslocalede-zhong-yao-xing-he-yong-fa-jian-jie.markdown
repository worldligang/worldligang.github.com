---
layout: post
title: "NSLocale的重要性和用法简介"
date: 2015-07-07 22:33:47 +0800
comments: true
categories: iOS开发
keywords: NSLocale, NSLocale使用, NSLocale用法简介, NSLocale简介, 刚刚在线
description: 这个时候NSLocale的重要性就体现出来了。NSLocale作为大家都不常用的一个类，NSLocale类是将与国家和语言相关的信息进行简单的组合，包括货币、语言、国家等的信息。

---

股票还在努力的往下跌，怎么办呢？我这真是杞人忧天啊。谁说我不着急呢？我的模拟炒股都亏的过半了吧。你想想那些真真炒股的人该有多担心。

罢了罢了，我还是滚去撸代码，消灭bug吧。

##上百万APP你用过几个？

随着iPhone的日趋强盛，随着AppStore的不断壮大，海量的应用如潮水般涌来。据统计AppStore上的应用早已过120万，Google Play Store更是超于AppStore，达到150万。

知道120万、150万是什么概念吗？人生不过短短两三万天。也就是说，如果你一天用一个APP的话，AppStore够你用40辈子，Google Play Store够你用50辈子，前提是，你必须生下来就开始玩手机。

这么多应用，不知道有没有你的贡献。估计正在看文章的你也贡献了好几个了吧。AppStore里面也有我的贡献。😊

开发一个APP不是那么容易的一件事，有很多坑等着你去踩。今天我们就抛出一个格式化时间的坑。

##NSDateFormatter格式化时间

之前的文章，讲到过：[NSDate和NSString相互转换](http://www.superqq.com/blog/2015/06/26/nsdatehe-nsstringxiang-hu-zhuan-huan/)。里面提到过如何讲NSDate转化成NSString。代码如下：

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

运行起来，看看打印的内容：

	2015-07-07 22:08:57.422 TestDemo[6756:1555205] 2015-07-07 22:08:57

这样写是不是没有什么问题。看着应该是没什么问题，其实问题很大。但是如果你改一下系统设置：语言设置成印度尼西亚文，时间设置成12小时制。

大家应该知道如何去设置吧，不会设置的请不要告诉我你是一名[iOS开发工程师](http://www.superqq.com/blog/2015/06/25/ru-he-cheng-wei-%5B%3F%5D-ming-you-xiu-de-ioskai-fa-gong-cheng-shi/)。再次编译起来，看看打印内容如下：

	2015-07-07 22:09:14.928 TestDemo[6762:1555466] 2015-07-07 10.09.14 PM

是不是愣住了，怎么会这么奇葩，时间怎么是.呢？还有更奇葩的呢，你去设置设置其他语言试试，也许有更多的收获。

##如何正确的格式化时间

这也是我们这两天遇到的问题，跟用户几经沟通之后，终于抓到log，发现问题竟然是格式化导致的。怎么解决呢？

这个时候NSLocale的重要性就体现出来了。NSLocale作为大家都不常用的一个类，NSLocale类是将与国家和语言相关的信息进行简单的组合，包括货币、语言、国家等的信息。

所以很简单，我们把dateFormatter的locale属性改一下即可解决这个问题。将下面代码放在dateFormatter初始化之后：

	NSLocale *usLocale = [[NSLocale alloc] initWithLocaleIdentifier:@"en_US"];
    dateFormatter.locale = usLocale;

看看问题解决没有，编译一下：

	2015-07-07 22:20:08.411 TestDemo[6769:1556968] 2015-07-07 22:20:08
	
果然，问题得到完美解决了。

##NSLocale用法简介

###获取国家、货币、语言地区编码

既然谈到NSLocale，我们就来简单了解一下：

	+ ISOCountryCodes// 所有的ISO定义的国家地区编码
	+ ISOCurrencyCodes// 所有的ISO定义的货币编码
	+ ISOLanguageCodes// 所有ISO定义的语言编码

以上我们可以用NSLog打印出来看一看。

###监听用户本地化的设置信息

	FOUNDATION_EXPORT NSString * const NSCurrentLocaleDidChangeNotification NS_AVAILABLE(10_5, 2_0);

###获取当前系统设置语言的标识符


	[[NSLocale currentLocale] localeIdentifier];

还有很多关于NSLocale的用法，自己动手多尝试。还有一个小问题。

##NSLocale怎么读？

看了这么久的NSLocale了，你知道NSLocale怎么读吗？不要翻译噢，看看你读的到底对不对？