---
layout: post
title: "Objective-C相关Category的收集"
date: 2015-01-15 10:38:25 +0800
comments: true
categories: 源代码
keywords: Categories, Objective-C, Cocoapods, Category, iOS, iOS开发, 收集, 个人博客, 刚刚在线
description: "Objective-C相关Category的收集"
 
---

更多iOS开发相关技术请关注iOS开发微信公众号 iOS开发 ：

	iOSDevTip

Categories是给你得不到源码的classes增加功能的一种方法。 <a href="http://cocoacats.com"target="_blank"title="iOS开发">这个页面</a> 收集一些相关的Category，并且持续更新，你可以订阅关注。作者是Fille Åström，是@ IMGNRY的联合创始人和开发者。
 
感谢大家的反馈，如果你有任何想法、抱怨或者建议，可以给我发送邮件（fille@imgnry.com），也可以在<a href="twitter.com/bobmoff"target="_blank"title="iOS开发">推特</a>或者App.net上给我发信息（@bobmoff）。
 
参考阅读：
苹果官方文档：<a href="https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/CustomizingExistingClasses/CustomizingExistingClasses.html"target="_blank"title="iOS开发">Customizing Existing Classes </a> 
 
 
<a href="https://github.com/Julioacarrettoni/UIImageView_FaceAwareFill"target="_blank"title="iOS开发">UIImageView+FaceAwareFill</a>

这个类别使用了Aspect Fill内容模式，可以自动根据图像内容进行调整，当检测到人脸时，它会以脸部中心替代掉以图片的几何中心。
测试环境：Xcode 5.0，iOS 6.0以上
 
<a href="https://github.com/bendytree/Objective-C-RegEx-Categories"target="_blank"title="iOS开发">NSRegularEx+ObjCRegex</a> 

Objective-C-RegEx-Categories是NSRegularExpression的一个延展，它可以把Object-C中的很多正则表达式合并成一个，简化了代码。
这个库没有任何依赖性，适用于iOS 4+和OS X 10.7+。
 
<a href="https://github.com/nicklockwood/AutoCoding"target="_blank"title="iOS开发">NSObject+AutoCoding</a>

AutoCoding是一个NSObject的类目，提供了对NSCoding 和NSCopying的自动支持。
兼容ARC和non-ARC编译目标
支持iOS 7.0/Mac OS 10.9 (Xcode 5.0, Apple LLVM compiler 5.0)
 
<a href="https://gist.github.com/alvesjtiago/8123006"target="_blank"title="iOS开发">NSInvocation+SimpleCreation</a> 

创建invocations的简单方法
 
<a href="https://github.com/shaahin/SHPersian"target="_blank"title="iOS开发">NSString+SHPersian</a>

SHPersian是一个针对使用波斯语和阿拉伯语的iOS开发者提供的工具，包含了一个在Persian iOS app中为文本添加自定义外观的必需的类。
 
<a href="https://gist.github.com/billyohgren/7944887"target="_blank"title="iOS开发">UILabel+ContentSize</a>

在UILabel内计算内容的大小。

 
<a href="https://github.com/RuiAAPeres/UIViewController-Swizzled"target="_blank"title="iOS开发">UIViewController+Swizzled</a> 

记录UIViewController层次：包括你在视图控制器的名字，还有你进入层次的展示。
 
<a href="https://gist.github.com/maciekish/7772693"target="_blank"title="iOS开发">NSObject+Association</a>

你是不是一直希望将"userInfo"显示到UIAlertView上呢？通过Association这个category可以将任意的对象赋值给其它任意对象(从iOS3.1和mac os 10.6到最新的系统版本。)
 
 
<a href="https://gist.github.com/maciekish/6268142"target="_blank"title="iOS开发">NSHTTPCookieStorage+FreezeDry</a>

app重启时，清除UIWebView cookies。
 
<a href="https://github.com/erica/uidevice-extension"target="_blank"title="iOS开发">UIDevice+Hardware</a>

检测硬件设备的版本。
 
<a href="https://gist.github.com/aegzorz/6068741"target="_blank"title="iOS开发">NSObject+LogDealloc</a>

是一个NSObject category，对于内存泄露的跟踪非常有用
 
<a href="https://github.com/krzysztofzablocki/SFObservers"target="_blank"title="iOS开发">NSObject+SFObservers</a>

是对NSNotificationCenter and KVO的一个扩展，它能够自动移除观察者。
 
<a href="https://gist.github.com/maciekish/6052297"target="_blank"title="iOS开发">UIApplication+NetworkActivity</a>

UIApplication+NetworkActivity跟踪你最近进行过的网络操作，并管理NetworkActivityIndicator。
 
<a href="https://gist.github.com/aegzorz/5974444"target="_blank"title="iOS开发">UIView+Recursion</a>

以递归的方式遍历(查找)subview
 
<a href="https://gist.github.com/bobmoff/5967220"target="_blank"title="iOS开发">UIView+RoundedCorners</a>

使用图层蒙版为视图添加圆角
 
<a href="https://gist.github.com/bobmoff/5967180"target="_blank"title="iOS开发">UIView+Stacker</a>

Stack subviews是按照索引进行垂直排序的。主要用于——使用xib时，以及需要进行view布局时(显示/隐藏)——当基于外部数据。不过不能用于autolayout。.
    
<a href="https://github.com/carlbrown/RegexOnNSString"target="_blank"title="iOS开发">NSString+PDRegex</a>

简化正则表达式的使用

<a href="https://gist.github.com/maciekish/5947238"target="_blank"title="iOS开发">MKMapView+MoveLogo</a>

这个类目允许你移动MKMapView logo，即使你放其他东西在mapview上它仍能保持可见。如果隐藏了logo，那将不能通过App Store审核。已经在iOS 5-iOS 7上进行了测试。
 
<a href="https://github.com/azu/NSDate-Escort"target="_blank"title="iOS开发">NSDate+Escort</a>

NSDate-Escort是一个NSDate实用库，兼容NSDate-Extensions API
 
<a href="https://gist.github.com/danielphillips/1005520"target="_blank"title="iOS开发">UILabel+DynamicSizeMe</a>

调整UILabel来根据内容改变其框架
 
<a href="https://github.com/scalessec/Toast"target="_blank"title="iOS开发">UIView+Toast</a>

适用于iOS上的Android风格toas通知。
 
<a href="https://github.com/Nyx0uf/NYXImagesKit"target="_blank"title="iOS开发">UIImage+NYXImagesKit</a> 

NYXImagesKit是一个重组了多个有用的UIImage categories的iOS项目，可对图像/图片进行多个处理，比如筛选、模糊、优化、蒙版、调整大小、旋转以及保存等等。同时还提供了一个UIImageView子类从URL异步加载图片，并在下载完毕时展示图片。
 
<a href="https://github.com/martinjuhasz/MJPopupViewController"target="_blank"title="iOS开发">UIViewController+MJPopup</a> 

MJPopupViewController是一个 UIViewController Category，用于使用不同的过渡效果来把ViewController作为弹出视图进行展示。
 
  <a href="https://github.com/mattgemmell/MGImageUtilities"target="_blank"title="iOS开发">UIImage+MGImageUtilities</a>

MGImageUtilities展示两个UIImage category：UIImage+ProportionalFill和UIImage+Tint。你可以通过UIImage+ProportionalFill调整任意图片的尺寸，可以使用UIImage+Tint来为图片着色。
 
<a href="https://github.com/Cocoanetics/DTFoundation"target="_blank"title="iOS开发">MultipleObjects+DTFoundation</a>

DTFoundation集合了实用方法和category的扩展，逐渐演变成一个文档齐全的工具集，记录和测试代码以加快开发。
 
<a href="https://github.com/rs/SDWebImage"target="_blank"title="iOS开发">UIImageView+WebCache</a>

SDWebImage提供一个UIImageView类别，以支持加载管理源自网络的远程图片。具有异步加载、缓存管理、同一个URL下载次数控制和优化等特征。简单易用。
 
 <a href="https://github.com/bennyguitar/Colours"target="_blank"title="iOS开发">UIColor+Colours</a>

100组漂亮的预制的色彩和配色方案可以让你的iOS/OS X开发更轻松。

 <a href="https://github.com/ProjectDent/UIImage-PDFColoredImage"target="_blank"title="iOS开发">UIImage+PDFColoredImage</a>

UIImage-PDFColoredImage是一个UIImage扩展，可以把黑色的PDF image转换成任意尺寸任意颜色的UIImage，只需一行代码，简单易用。
 
<a href="https://github.com/k06a/NSData-AsyncCacher"target="_blank"title="iOS开发">NSData+AsyncCacher</a>

NSData-AsyncCacher是NSData的一个category，用于从url和block中异步加载数据。请求数据使用NSCache缓存，可以多次请求。
 
 <a href="https://github.com/billymeltdown/nsdate-helper"target="_blank"title="iOS开发">NSDate+Helper</a>

通过一些便捷函数扩展了Cocoa的NSDate
 
 <a href="https://github.com/supermarin/ObjectiveSugar"target="_blank"title="iOS开发">MultipleObjects+ObjectiveSugar</a>

Objective-C additions for humans. Ruby style.
 
<a href="https://github.com/Kjuly/MKMapView-ZoomLevel"target="_blank"title="iOS开发">MKMapView+ZoomLevel</a>

在MapView中设置缩放比例。
 
 <a href="https://gist.github.com/Abeansits/5848341"target="_blank"title="iOS开发">NSString+Extensions</a>

一个针对NSStrings的category，它能将string转换为SHA1，NSNumber。检测string是否为空、是否包含某个子字符串，以及替换掉NSDictionary中的子字符串。
 
<a href="https://github.com/nicklockwood/ViewUtils"target="_blank"title="iOS开发">UIView+Utils</a>

ViewUtils是一个category methods的集合，这些方法是UIView的扩展，包含了许多方便的属性和方法，其中许多都是我们希望苹果内置的。
 
 <a href="https://github.com/kevinlawler/NSDate-TimeAgo"target="_blank"title="iOS开发">NSDate+TimeAgo</a>

让NSDate报告时间，比如"A moment ago", "30 seconds ago", "5 minutes ago", "Yesterday", "Last month", "2 years ago"以及其他。
 
 <a href="https://github.com/alexdrone/ios-fontawesome"target="_blank"title="iOS开发">NSString+FontAwesome</a>

FontAwesome+iOS是一个开源形象字体库，通过扩展 NSString 让你在app中轻松使用 FontAwesome 字体。
官方网站：http://fortawesome.github.io/Font-Awesome/
 
 <a href="https://gist.github.com/aegzorz/5797393"target="_blank"title="iOS开发">NSArray+FirstObject</a>

从数组中获得第一个对象，或者从空数组中返回零
 
  <a href="https://github.com/djmadcat/NSObject-AutoDescription"target="_blank"title="iOS开发">NSObject+AutoDescription</a>

能够简单地为任何类描述日志(从 NSObject派生出来)
 
  <a href="https://gist.github.com/aegzorz/5797337"target="_blank"title="iOS开发">CGRect+Additions</a>

处理CGRects的一些函数。
 
 <a href="https://gist.github.com/bobmoff/5276954"target="_blank"title="iOS开发">UIView+ModifyFrame</a>

简单实用的UIView类目使修改框架不那么变态。
 
<a href="https://gist.github.com/dsibilly/1038500"target="_blank"title="iOS开发">NSString+UsefulShit</a>

Objective-C category 示范
