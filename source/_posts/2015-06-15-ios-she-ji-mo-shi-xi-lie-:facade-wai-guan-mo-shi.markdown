---
layout: post
title: "iOS 设计模式系列：Facade – 外观模式"
date: 2015-06-15 10:06:20 +0800
comments: true
categories: iOS开发
keywords: 设计模式, MVC, 单例模式, 外观模式, 装饰器模式, 适配器模式, 观察者模式,备忘录模式,归档模式,命令模式, cocoapods, 个人博客, 刚刚在线
description: "iOS 设计模式系列：MVC、Singleton – 单例模式、Facade – 外观模式、Decorator – 装饰器模式、Adapter – 适配器模式、Observer – 观察者模式、Memento – 备忘录模式、Archiving – 归档模式、Command – 命令模式"

---

目前你有 PersistencyManager 来在本地存储专辑数据，HTTPClient 处理远程通信。项目中其它的类跟这些逻辑都没关。

执行这个[模式](http://www.superqq.com/blog/2015/03/21/dan-li-mo-shi-de-xie-fa/)，只有 LibraryAPI 来保存 PersistencyManager 和 HTTPClient 的实例。之后，LibraryAPI 将会公开一个简单的 API 来访问这些服务。

	提示：通常，单例存在 app 的整个生命周期。不需一直使用很多指针指向其它对象，因为她们直到程序关闭后才会被释放。   

这个设计就像下图：

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2013/08/design-patterns-facade-uml-480x71.png)

LibraryAPI 将会公开给其它代码，但是它隐藏了 APP 中 HTTPClient 和 PersistencyManager 的复杂部分。

打开 LibraryAPI.h，在顶部引入面文件：

	#import "Album.h"

接下来，在 LibraryAPI.h下面添加如下方法：

	- (NSArray*)getAlbums;
	- (void)addAlbum:(Album*)album atIndex:(int)index;
	- (void)deleteAlbumAtIndex:(int)index;

现在，这些方法都公开给了其它类。

在 LibraryAPI.m 文件引入如下两个文件：

#import "PersistencyManager.h"
#import "HTTPClient.h"

只有在这个地方你才会需要引入这些类。记住：你的 API 将会是你「复杂」系统的唯一的接入点。

现在添加一些私有属性在你的类的扩展里（在 @implementation 上面）

	@interface LibraryAPI () {
	    PersistencyManager *persistencyManager;
	    HTTPClient *httpClient;
	    BOOL isOnline;
	}
	@end

isOnline 用来判断，如果专辑列表数据发生变化是否能够更新到服务器，例如添加或者删除专辑。

你现在需要在 init 方法中初始化这些变量，在 LibraryAPI.m 中添加下面代码：

	- (id)init
	{
	    self = [super init];
	    if (self) {
	        persistencyManager = [[PersistencyManager alloc] init];
	        httpClient = [[HTTPClient alloc] init];
	        isOnline = NO;
	    }
	    return self;
	}

这个 HTTP 客户端在这里并不真正的工作，它只是在外观设计里面起一个示范用法的作用，所以 isOnline 永远是 NO 了。

接下来，在 LibraryAPI.m 里面添加下面三个方法：

	- (NSArray*)getAlbums
	{
	    return [persistencyManager getAlbums];
	}
	
	- (void)addAlbum:(Album*)album atIndex:(int)index
	{
	    [persistencyManager addAlbum:album atIndex:index];
	    if (isOnline)
	    {
	        [httpClient postRequest:@"/api/addAlbum" body:[album description]];
	    }
	}
	
	- (void)deleteAlbumAtIndex:(int)index
	{
	    [persistencyManager deleteAlbumAtIndex:index];
	    if (isOnline)
	    {
	        [httpClient postRequest:@"/api/deleteAlbum" body:[@(index) description]];
	    }
	}

看一下 addAlbum:atIndex:。这个类首先更新本地数据，如果联网，它再更新远端服务器。这就是外观设计的长处；当一些系统外的类添加了一个新专辑，它不知道─也不需要知道─复杂的内部系统。

	提示：当在你的子系统里设计一个外观类的时候，记住没有任何东西可能阻止客户访问这些「隐藏」类。要多写些防御性的代码，不要想当然的认为所有客户都会用同样的方式使用你的外观类。

运行你的程序，你会看一个黑底空白内容的屏幕，像下面这样：

![icon1](http://cdn1.raywenderlich.com/wp-content/uploads/2013/09/2013-09-01_12-08-44-211x320.png)

你需要在屏幕上显示一些专辑数据─这就需要使用到下面一个设计模式：装饰器 (Decorator)。

**设计模式系列文章**：

[iOS 设计模式系列：开篇](http://www.superqq.com/blog/2015/06/10/ios-she-ji-mo-shi-xi-lie-:kai-pian/)

[iOS 设计模式系列：MVC – 设计模式中的国王](http://www.superqq.com/blog/2015/06/11/ios-she-ji-mo-shi-xi-lie-:mvc-she-ji-mo-shi-zhong-de-guo-wang/)

[iOS 设计模式系列：Singleton – 单例模式](http://www.superqq.com/blog/2015/06/13/ios-she-ji-mo-shi-xi-lie-:singleton-dan-li-mo-shi/)

[iOS 设计模式系列：Facade – 外观模式](http://www.superqq.com/blog/2015/06/15/ios-she-ji-mo-shi-xi-lie-:facade-wai-guan-mo-shi/)