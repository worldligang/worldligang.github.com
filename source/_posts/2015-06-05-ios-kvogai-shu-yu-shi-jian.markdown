---
layout: post
title: "iOS KVO概述"
date: 2015-06-05 11:06:59 +0800
comments: true
categories: iOS面试
keywords: iOS KVO, KVO, iOS开发, iOS, 面试,  iOS面试, cocoapods, 个人博客, 刚刚在线
description: "iOS KVO概述与实践，KVO是什么？"

---

>面试中经常会被问到：什么是KVO？这个问题既然出现概率这么大，那么我们就来详细讲一讲到底什么是KVO。下次再有面试官问你的时候，你就可以娓娓道来，以彰显高逼格

##概述

问：什么是KVO？

答：KVO是Key-Value Observing的缩写。它提供一种机制，当指定的对象的属性被修改后，则对象就会接受到通知。简单的说就是每次指定的被观察的对象的属性被修改后，KVO就会自动通知相应的观察者了。

如果你能这样回答，面试官一定不会说你错的，这就是KVO的原理。但是如果你能说出KVO的好处，那一定能赢得面试官的刮目相看。比如：

* 使用KVO最直接的好处就是可以减少代码量。
* KVO是**观察者**设计模式中的一种，有利于业务逻辑于视图控制之间的解耦。

更深刻的了解KVO，你可以动手写一个小demo思路如下：

* 定义一个对象People，分别有name和age属性
* 监听People的age属性
* 定义一个UIButton，在button的点击方法里面，去改变People的age。
* 你就可以收到age改变时发出来的通知
* 在对象销毁的时候，移除通知。


iOS面试汇总：

[面试官问现在工资是多少该怎么回答?](http://www.superqq.com/blog/2015/06/04/mian-shi-guan-wen-xian-zai-gong-zi-shi-duo-shao-gai-zen-yao-hui-da/)


* 关注公众号iOS开发：**iOSDevTip** 获取更多面试技巧。

