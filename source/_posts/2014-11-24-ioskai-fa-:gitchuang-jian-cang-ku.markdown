---
layout: post
title: "iOS开发：git创建仓库"
date: 2014-11-24 09:17:38 +0800
comments: true
categories: iOS开发
keywords: iOS, git, 版本, 仓库, repository, git reflog, iOSDevTip iOS开发, gcd, cocoapods, 队列, 个人博客, 刚刚在线
description: "iOS开发：git创建仓库"

---

从 Git 中移除某个文件

	rm test.txt

	git rm test.txt


添加远程仓库

	$ git remote origin $ git remote add pb git://github.com/paulboone/ticgit.git $ git remote -v origin  git://github.com/schacon/ticgit.git pb  git://github.com/paulboone/ticgit.git

远程仓库的删除和重命名

	$ git remote rename pb paul $ git remote origin paul


碰到远端仓库服务器迁移，或者原来的克隆镜像不再使用，又或者某个参与者不再贡献代码，那么需要移除对应的远端仓库，可以运行 git remote rm 命令：

	$ git remote rm paul $ git remote origin 


我的微信公众号：
	
	iOSDevTip


