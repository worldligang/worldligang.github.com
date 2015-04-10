---
layout: post
title: " iOS开发中git分支创建和管理"
date: 2014-11-19 10:19:00 +0800
comments: true
categories: iOS开发
keywords: iOS, git,git分支, 版本, 仓库, repository, git reflog, iOSDevTip iOS开发, gcd, cocoapods, 队列, 个人博客, 刚刚在线
description: "iOS开发中git分支创建和管理" 

---

<a href="http://www.superqq.com/">iOS开发中git分支创建和管理</a>

创建分支


	$ git branch testing


切换到testing分支


	$ git checkout testing


查看本地分支


	$ git branch


查看远程分支


	$ git branch -a


删除本地分支


	$ git branch -D testing




提交本地分支到远程


1，切换到要提交的分支


	$ git checkout testing


2，提交到远程


	$ git push origin testing


删除远程分支


	$git push origin :branch-name


冒号前面的空格不能少，原理是把一个空分支push到server上，相当于删除该分支。

iOS开发微信公众号 iOS开发 ：

	iOSDevTip