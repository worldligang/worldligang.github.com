---
layout: post
title: "iOS KVC详细讲解"
date: 2015-06-06 09:20:27 +0800
comments: true
categories: 程序员
keywords: iOS KVC, KVC, iOS开发, iOS, 面试,  iOS面试, cocoapods, 个人博客, 刚刚在线
description: "iOS KVC详细讲解,关于KVC的知识"

---

>欢迎大家提供面试题目，我们一起来讲解，以备不时之需。

##什么是KVC？

KVC即**NSKeyValueCoding**，就是键-值编码的意思。一个非正式的 Protocol，是一种间接访问对象的属性使用字符串来标识属性，而不是通过调用存取方法，直接或通过实例变量访问的机制。

以上就是KVC的理论，面试时，如实回答就可以啦！

##使用KVC说明

* KVC间接修改对象属性时，会自动判断对象属性的类型，完成相应的转换。
* KVC按键值路径取值时，如果对象不包含指定的键值，那么就会自动进入对象内部，查找对象属性。
* KVC可以嵌套按照键值路径取值。

##KVC和KVO有什么区别？


上一篇文章我们讲了[iOS KVO概述](http://www.superqq.com/blog/2015/06/05/ios-kvogai-shu-yu-shi-jian/)，有兴趣的可以看看。面试过程中，面试官问：KVC和KVO有什么区别？

听到这个问题真的很（dan）疼，真特么想回一句，我怎么知道什么区别？当然，如果你像这么回答，面试估计到此结束啦。

那KVC和KVO到底有什么区别呢？

KVO是建立在KVC之上的，KVO能够观察一个对象的KVC key-path值的变化。

iOS面试汇总：

[面试官问现在工资是多少该怎么回答?](http://www.superqq.com/blog/2015/06/04/mian-shi-guan-wen-xian-zai-gong-zi-shi-duo-shao-gai-zen-yao-hui-da/)

[iOS KVO概述](http://www.superqq.com/blog/2015/06/05/ios-kvogai-shu-yu-shi-jian/)


* 关注公众号iOS开发：**iOSDevTip** 获取更多面试技巧。