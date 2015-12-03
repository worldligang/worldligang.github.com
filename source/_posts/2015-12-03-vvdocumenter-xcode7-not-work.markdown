---
layout: post
title: 升级Xcode7之后VVDocumenter-Xcode不能用的解决办法
date: 2015-12-03 22:24:57 +0800
comments: true
categories: iOS开发
keywords: VVDocumenter, 系统设置,系统设置里的WiFi界面 , iOS开发, 刚刚在线
description: VVDocumenter-Xcode是Xcode上一款快速添加标准注释，并可以自动生成文档的插件。有了VVDocumenter-Xcode，规范化的注释，只需要输入三个斜线“///”就可以搞定，非常方面实用，目前已经支持Swift。

---


VVDocumenter-Xcode是Xcode上一款快速添加标准注释，并可以自动生成文档的插件。有了VVDocumenter-Xcode，规范化的注释，只需要输入三个斜线“///”就可以搞定，非常方面实用，目前已经支持Swift。

Objective-C效果图：

![VVDocumenter1](http://images.90159.com/11/vvdocumenter1.gif)

Swift效果图：

![VVDocumenter1](http://images.90159.com/11/vvdocumenter2.gif)

从Xcode 5开始，苹果要求加入UUID证书从而保证插件的稳定性。因此Xcode版本更新之后需要在VVDocumenter-Xcode的Info.plist文件中添加Xcode的UUID。

##步骤如下：

###一、查看Xcode的UUID

####方式1

在终端执行 

	defaults read /Applications/Xcode.app/Contents/Info DVTPlugInCompatibilityUUID
	
![VVDocumenter1](http://images.90159.com/11/vvdocumenter3.jpg)

	
拷贝选中的字符串。


####方式2

在/Applications目录中找到Xcode.app，右键”显示包内容”，进入Contents文件夹，双击Info.plist打开，找到DVTPlugInCompatibilityUUID，拷贝后面的字符串。

###二、添加Xcode的UUID到VVDocumenter-Xcode的Info.plist文件

####方式1--插件已经安装完成

1、打开xcode插件所在的目录：~/Library/Application Support/Developer/Shared/Xcode/Plug-ins；

2、选择已经安装的插件例如VVDocumenter-Xcode，右键”显示包内容”；

3、找到info.plist 文件，找到DVTPlugInCompatibilityUUIDs的项目，添加一个Item，Value的值为之前Xcode的UUID，保存。

![VVDocumenter1](http://images.90159.com/11/vvdocumenter4.jpg)


####方式2--插件还未安装/重新安装

1、从GitHub克隆仓库到本地，在Xcode中打开项目，选择项目名称，在TAGETS下选中VVDocumenter-Xcode；

2、选择Info，找到DVTPlugInCompatibilityUUIDs的项目，添加一个Item，Value的值为之前Xcode的UUID；

3、Build项目，VVDocumenter-Xcode会自动安装。

###三、重启Xcode

Xcode 6之后，重启Xcode时会提示“Load bundle”、 “Skip Bundle”，这里必须选择“Load bundle”，不然插件无法使用。