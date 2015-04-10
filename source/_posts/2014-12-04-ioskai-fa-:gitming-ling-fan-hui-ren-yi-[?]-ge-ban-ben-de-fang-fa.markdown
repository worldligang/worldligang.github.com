---
layout: post
title: "iOS开发：git命令返回任意一个版本的方法"
date: 2014-12-04 18:28:04 +0800
comments: true
categories: iOS开发
keywords: iOS, git, 版本, git log, git reflog, iOSDevTip iOS开发, gcd, cocoapods, 队列, 个人博客, 刚刚在线
description: "iOS开发：git命令返回任意一个版本的方法"

---

git回退某一个版本的方法：


<div class="entry-content"><h3>一：记录回退的hash值</h3>


首先要查看你要回退到哪一个版本

方法1：git log

会展示最新的提交记录，然后你按着键盘的“向下”键，看更多的提交记录，记住你要回退的哪一个版本的hash值


方法2：git reflog

会展示最近操作的记录，然后你记住要回退的hash值


<div class="entry-content"><h3>二：执行回退命令</h3>

知道hash值之后，接下来就是指向回退命令了：

git reset --hard +版本号回到该版本状态下


这样你就可以回退的任意一个版本了，挺方便的。

</br>

更多iOS开发技术请关注：

微信公众号：

	iOSDevTip