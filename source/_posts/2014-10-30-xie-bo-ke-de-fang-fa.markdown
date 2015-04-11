---
layout: post
title: "Octopress写博客的方法"
date: 2014-09-30 11:34:57 +0800
comments: true
categories: iOS开发
keywords: iOS, 写博客, Octopress, rake new_post, iOSDevTip iOS开发, gcd, cocoapods, 队列, 个人博客, 刚刚在线
description: "Octopress写博客的方法" 

---
<a href="http://www.superqq.com/blog/2014/09/30/xie-bo-ke-de-fang-fa/">写博客的方法</a>

写博客的方法:

    rake new_post[‘article name’] 生成博文框架，填写文章标题
    open -a Mou source/_posts/2014-10-30-xie-bo-ke-de-fang-fa.markdown  打开markdown编辑工具（比较好用，下载地址：http://25.io/mou/）
    rake generate 生成静态文件
    rake watch 检测文件变化，实时生成新内容
    rake preview 本地预览“http://localhost:4000”
    rake deploy 发布文章


微信公众账号：iOS开发
	
	iOSDevTip
	
	
原文链接：<a href="http://blog.leichunfeng.com/blog/2014/11/11/use-octopress-plus-github-pages-to-setup-a-personal-blog/">使用 Octopress+GitHub Pages 搭建个人博客</a> 