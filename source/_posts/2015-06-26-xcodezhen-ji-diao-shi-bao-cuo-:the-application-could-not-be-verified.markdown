---
layout: post
title: " Xcode真机调试报错：The application could not be verified."
date: 2015-06-26 15:33:34 +0800
comments: true
categories: iOS开发
keywords: The application could not be verified., Xcode真机调试报错,  刚刚在线
description: Xcode真机调试报错：The application could not be verified.

---

 今天真机调试的时候遇到这个错误：
 
 	The application could not be verified.
 	
 这还是第一次遇到，应该是手机上的app的证书跟现在的证书不一致导致。
 
 解决方法有两个
 
 
 ###xcode中切换证书：
 
 
 
 	你手机上的app用的是哪个证书，你现在还用那个证书运行。
 
 
 
 ###删除手机上的app
 
 
	直接删除手机上的app，再运行就可以啦！