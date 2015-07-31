---
layout: post
title: "为刚刚在线（Octopress）添加多说评论系统"
date: 2015-08-01 01:04:31 +0800
comments: true
categories: 程序员
keywords: 网站, iOS李刚, 李刚iOS, iOS开发, 刚刚在线
description: 我的网站刚刚在线去年九月份正式上线的，马上快一周年啦！作为一个技术型的博客网站，刚刚在线成长速度还是蛮快的。

---

>今天是八一建军节，中国人民解放军建军纪念日。心里不由自主地升起一份爱国之情。

我的网站[刚刚在线](http://www.superqq.com/)去年九月份正式上线的，马上快一周年啦！作为一个技术型的博客网站，[刚刚在线](http://www.superqq.com/)成长速度还是蛮快的。

##刚刚在线排名

特别是最近一段时间，我一直在关注[刚刚在线](http://www.superqq.com/)的排名，每天的进步速度非常快。这也是我持续更新的结果，百度对持续更新的网站是非常友好的。

我用Alexa的插件看了一下[刚刚在线](http://www.superqq.com/)的排名，全球排名：**240334** ， 中国区排名：**14665** 。我特意截了一张图给大家看一看：

![superqqalexa.jpg](http://7xkkk9.com1.z0.glb.clouddn.com/superqqalexa.jpg)

路漫漫其修远兮，吾将上下而求索。要想取得更好的成就，仍需努力！[刚刚在线](http://www.superqq.com/)一直有一个问题困惑着我。就是在火狐浏览器中，文章底部评论模块会出现错乱。

##火狐评论版块错乱修复

[刚刚在线](http://www.superqq.com/)的评论模块用的是友言。也挺奇怪的，在其他浏览器中就好好的，唯独在火狐浏览器了会出现错乱。我知道这个问题的存在，已有一些访客反应过这个问题。但是，我一直没有找到原因。

估计iOS开发者用火狐浏览器的人肯定不少，这个错乱问题一直在这里的话，肯定会给访客留下不好的印象。所以得尽早解决这个问题。

既然找不到解决错乱的方法，就把友言给替换掉吧。百度搜了一下，发现国内的还有多说，评论功能还是挺不错的。我也看到很多网站在用多说。

##把友言替换成多说

说换就换，多说（ **http://duoshuo.com/** ）接入流程也不复杂。网上有很多教程，我简单说一下我是如何操作的。

###1.获取站点的short_name

到多说的官网注册账号，并且添加你的站点。`short_name`就是添加站点的时候的多说域名框里填写的。添加站点流程如下：

![duoshuozhuce.jpg](http://7xkkk9.com1.z0.glb.clouddn.com/duoshuozhuce.jpg)

###2.配置_config.yml

在 `_config.yml` 中添加

	# duoshuo comments
	duoshuo_comments: true
	duoshuo_short_name: superqq

具体配置方法请看点击[ 为 Octopress 添加多说评论系统 ](http://blog.csdn.net/iosdevtip/article/details/47179709)
 
ok，经过上面五小步就可以实现接入多说评论版块啦。迫不及待地想看看问题有没有解决，下过如何？赶紧部署上去，我简单的测试了一下。你可以点击[刚刚在线](http://www.superqq.com/)，看看实现效果：
 
![duoshuocomment.jpg](http://7xkkk9.com1.z0.glb.clouddn.com/duoshuocomment.jpg)
  
##网站有评论必回复噢

既然网站评论已经成功接入，大家以后有什么问题，或者需要交流的可以直接在网站上评论。虽然，每天有很多少要做，但是我依然会抽出时间来看看大家提的评论，一起探讨，一起成长。