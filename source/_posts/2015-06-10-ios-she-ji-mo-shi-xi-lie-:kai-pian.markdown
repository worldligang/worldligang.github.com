---
layout: post
title: "iOS 设计模式系列：开篇"
date: 2015-06-10 15:10:13 +0800
comments: true
categories: iOS开发
keywords: 设计模式, MVC, 单例模式, 外观模式, 装饰器模式, 适配器模式, 观察者模式,备忘录模式,归档模式,命令模式, cocoapods, 个人博客, 刚刚在线
description: "iOS 设计模式系列：MVC、Singleton – 单例模式、Facade – 外观模式、Decorator – 装饰器模式、Adapter – 适配器模式、Observer – 观察者模式、Memento – 备忘录模式、Archiving – 归档模式、Command – 命令模式"

---

原文地址：[iOS Design Patterns](http://www.raywenderlich.com/46988/ios-design-patterns)

原文作者：Eli Ganem

> 有些专业词汇很难翻译准确，就不翻译了。如果英文阅读没有障碍，还是鼓励大家都读原作。

iOS 设计模式 – 你大概经常听过这样的术语，但是你知道它的真正意思吗？虽然大部分开发者都同意设计模式很重要，但是关于这方面的文章却很少，开发者在写代码的时候，有时候并不会去花很多时间去考虑设计模式。

设计模式的功能是在软件设计当中是解决一些重复的公共问题。他们是一些模板来帮助你更容易的书写代码和复用你的代码。他们还可能帮助你创建低耦合的代码，你可以很轻松的修改和替换其中的组件。

如果你已经很熟习设计模式了，我这里有一些好的建议。首先，你已经使用了大量由 Cocoa 创建 iOS 设计模式，同时最好的练习就是去使用她们。第二，这个教程将带给你全面了解所有最新的 iOS 设计模式，她们一般都是由 Cocoa 编写的。

这个教程被分成几个小节。在每一个小节里， 你都能在下面读到说明：

* 什么是设计模式
* 为什么你应该用她
* 怎么用，在哪里用，在使用当中如何注意一些常见的陷阱

在这个教程中，你将要创建一个音乐库的 APP，它能够显示你的音乐专辑和一些相关信息。

在开发这个 APP 的过程当中，你将会了解到很多常见的 Cocoa 设计模式。

* 创建 (Creational)：单例 (Singleton)和抽象工厂 (Abstract Factory)
* 结构 (Structural)：MVC，装饰器 (Decorator)，适配器 （Adapter)，外观 (Facade)和复合器 (Composite)
* 行为 (Behavioral)：观察者 (Observer)，备忘录 (Memento)，责任链 (Chain of Responsibility)和命令 (Command)

不要被带偏了，这只是一篇理论文章；你将会在你的音乐 APP 中使用很多的设计模式。在这个教程的最后，你的 APP 看来应该是像这个样子的：

![icon](http://cdn1.raywenderlich.com/wp-content/uploads/2013/07/finalapp-180x320.png)

###入门指南

下载 <a href="http://cdn2.raywenderlich.com/wp-content/uploads/2013/07/BlueLibrary-Starter.zip" target="_blank" title="启动项目">启动项目</a>，从 ZIP 文件中提取文件，用 Xcode 打开 BlueLibrary.xcodeproj。

这没有多少文件，仅仅只有一个默认的 ViewController 文件和一个空的可执行的简单 HTTP 客户端。

 
 	提示：你知道吗，当你创建一个新的 Xcode 项目的时候，你的代码已经使用被设计模式？MVC，代理，协议，单例模式 ─ 这些你都能免费得到！:]


在你开始研究第一个设计模式时，你必须创建两个类用于存储和显示专集数据。

从”File\New\File…”（或者使用快捷键 Command + N）。选择 iOS > Cocoa Touch，然后选择 Objective-C class 后点击 Next。设置类的名称为 Album，子类为 NSObject。点击 Next 创建。

打开 Album.h 文件，在 @interface 和 @end 这间添加下面的属性和方法原型：

	@propery (nonatomic, copy, readonly) NSString *title, *artist, * genre, *coverUrl, *year;
	- (id)initWithTitle:(NSString *)title artist:(NSString *)artist coverUrl:(NSString *)coverUrl year:(NSString *)year;

注意这里的所有属性都是 readonly，Album 对象被创建后是不能够被修改的。

这个方法是用来初始化对象的。当你创建一个新专辑时，需要提供专辑名称，作者，封面 URL 和年份。

现在打开 Album.m 文件然后在 @implementation 和 @end 之间加入如下代码：

	- (id)initWithTitle:(NSString*)title artist:(NSString*)artist coverUrl:(NSString*)coverUrl
	{
	    self = [super init];
	    _title = title;
	    _artist = artist;
	    _coverUrl = coverUrl;
	    _year = year;
	    _genre = @"Pop";
	    return self;
	}

这里没有什么复杂的，只是一个简单的创建 Album 实例的初始化方法。

现次从菜单 File\New\File 选择中 Cocoa Touch，然后选择 Object-C class，点击 Next。设置类的名称为 AlbumView，子类为 UIView。点击 Next 创建文件。

	提示：如果你会使用快捷键，创建这些会更容易，Command+N 创建新文件，Command+Option+N 创建新的文件夹，Command+B 创建新项目，Command+R 运行项目。

打开 AlbumView.h，在 @interface 和 @end 中 添加一个方法原型

	- (id)initWithFrame:(CGRect)fram albumCover:(NSString*)albumCover;

现在打开 AlbumView.m 文件，用下面代码替换 @implementaton 后面的所有代码：

	@implementation AlbumView
	{
	    UIImageView *coverImage;
	    UIActivityIndicatorView *indicator;
	}
	
	- (id)initWithFrame:(CGRect)frame albumCover:(NSString*)albumCover
	{
	    self = [super initWithFrame:frame];
	    if (self) {
	        self.backgroundColor = [UIColor blackColor];
	        // the coverImage has a 5 pixels margin from its frame
	        coverImage = [[UIImageView alloc] initWithFrame:CGRectMake(5, 5, frame.size.width-10, frame.size.height-10)];
	        [self addSubview:coverImage];
	
	        indicator = [[UIActivityIndicatorView alloc] init];
	        indicator.center = self.center;
	        indicator.activityIndicatorViewStyle = UIActivityIndicatorViewStyleWhiteLarge;
	        [indicator startAnimating];
	        [self addSubview:indicator];
	        }
	    return self;
	}

	@end

第一个需要注意的地方是 coverImage 是一个实例变量。这个变量代表专集的封面图片。第二个变量是一个提示器，在封面图片开始下载的时候提示用户正在下载。

在初始化的过程中你需要设置背景为黑色，创建一个比封面图片大 5px 的图片视图，再创建和添加一个活动指示器。

	提示：为什么私有变量定义在执行文件而不是定义在接口文件？这是因为其它外部类不需要知道 AlbumView 类里的这些变量，这些变量只会在这个类的内部方法中使用到。如果你要开发一些库或者框架给别的开发者使用，遵守这个习俗是非常重要的。

Build 你的项目确保所有代码都能构运行。没有问题？你的第一个设计模式已经准备好了。

**设计模式系列文章**：

[iOS 设计模式系列：开篇](http://www.superqq.com/blog/2015/06/10/ios-she-ji-mo-shi-xi-lie-:kai-pian/)

[iOS 设计模式系列：MVC – 设计模式中的国王](http://www.superqq.com/blog/2015/06/11/ios-she-ji-mo-shi-xi-lie-:mvc-she-ji-mo-shi-zhong-de-guo-wang/)

[iOS 设计模式系列：Singleton – 单例模式](http://www.superqq.com/blog/2015/06/13/ios-she-ji-mo-shi-xi-lie-:singleton-dan-li-mo-shi/)

[iOS 设计模式系列：Facade – 外观模式](http://www.superqq.com/blog/2015/06/15/ios-she-ji-mo-shi-xi-lie-:facade-wai-guan-mo-shi/)

