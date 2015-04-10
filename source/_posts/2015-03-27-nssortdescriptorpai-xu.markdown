---
layout: post
title: "NSSortDescriptor排序"
date: 2015-03-27 19:01:45 +0800
comments: true
categories: iOS开发
keywords: NSSortDescriptor, LGPeople, 排序,  升序, iOS开发, cocoapods, 个人博客, 刚刚在线
description: "NSSortDescriptor排序"

---


NSSortDescriptor可以实现按照对象的属性进行排序。比如我们有一个对象（LGPeople）。LGPeople有两个属性分别是名字（name）和年龄（age）。现在我们要按照LGPeople的年龄（age）来进行排序。

代码如下：
		
		//按年龄排序
        NSSortDescriptor *sortDescriptor = [[NSSortDescriptor alloc] initWithKey:@"_age" ascending:NO];
        NSArray *sortDescriptors = [NSArray arrayWithObjects:sortDescriptor, nil];
        [arrayPeople sortUsingDescriptors:sortDescriptors];
        
这样是按照年龄的降序进行排序。如果想升序的话将NO改为YES即可。如果是按照名字进行排序的话也很简单：

		//按姓名排序
        NSSortDescriptor *sortDescriptorName = [[NSSortDescriptor alloc] initWithKey:@"_name" ascending:NO];
        NSArray *sortDescriptors = [NSArray arrayWithObjects:sortDescriptorName, nil];
        [arrayPeople sortUsingDescriptors:sortDescriptors];

也许我们还有一种需求：先按照年龄排序，再按名字进行排序。代码如下：
		
		//按年龄和名字排序
        NSSortDescriptor *sortDescriptor = [[NSSortDescriptor alloc] initWithKey:@"_age" ascending:NO];
        NSSortDescriptor *sortDescriptorName = [[NSSortDescriptor alloc] initWithKey:@"_name" ascending:NO];
        NSArray *sortDescriptors = [NSArray arrayWithObjects:sortDescriptor, sortDescriptorName, nil];
        [_mutArrVideo sortUsingDescriptors:sortDescriptors];
        
这样相同年龄的情况下，就会按照名字进行排序。当然还可以设置两个以上的排序条件。

	
更多iOS开发相关技术请关注iOS开发微信公众号 iOS开发 ：

	iOSDevTip