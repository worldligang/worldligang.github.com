---
layout: post
title: "iOS 设计模式系列：Singleton – 单例模式"
date: 2015-06-13 08:21:55 +0800
comments: true
categories: iOS开发
keywords: 设计模式, MVC, 单例模式, 外观模式, 装饰器模式, 适配器模式, 观察者模式,备忘录模式,归档模式,命令模式, cocoapods, 个人博客, 刚刚在线
description: "iOS 设计模式系列：MVC、Singleton – 单例模式、Facade – 外观模式、Decorator – 装饰器模式、Adapter – 适配器模式、Observer – 观察者模式、Memento – 备忘录模式、Archiving – 归档模式、Command – 命令模式"

---

[单例设计模式](http://www.superqq.com/blog/2015/03/21/dan-li-mo-shi-de-xie-fa/)确切的说就是一个类只有一个实例，有一个全局的接口来访问这个实例。当第一次载入的时候，它通常使用延时加载的方法创建单一实例。

	提示：苹果大量的使用了这种方法。例子：[NSUserDefaults standerUserDefaults], [UIApplication sharedApplication], [UIScreen mainScreen], [NSFileManager defaultManager] 都返回一个单一对象。

你可能想知道你为什么要关心一个类有多个的实例。代码和内存都很便宜，不是吗？

在一些情况下，一个类只有一个实例是有意义的。例如，这里没有必要有多个登录实例，除非你一次想写入多个日志文件。或者，一个全局的配置类文件：它可以很容易的很安全的执行一个公共资源，这样的一个配置文件，要比同时修改多个配置类文件好很多。

###如何使用单例模式

请看下面的图片

![icon](http://cdn3.raywenderlich.com/wp-content/uploads/2013/08/singleton.png)

上面的图片显示的是一个登录类，它有一个属性（这个单一实例），有两个方法：sharedInstance 和 init。

首先一个客户端(client)发送 sharedInstance 信息，但是属性 instance 还没有初始化，所以你要先给这个类创建一个实例。

然后你调用 sharedInstance，instance 会马上返回初始化的值。这个逻辑最终只会返回一个实例。

你需要执行这个模式来创建单例类来管理所有的专辑数据。

你需要注意在项目里有一个叫 API 文件夹，给你的 APP 提供服务的所有类都需要放在这里。在这个文件夹里用 iOS\Cocoa Touch\Object-C class 创建一个新类。类的名字叫 LibraryAPI，子类选择 NSObject。

打开 LibraryAPI.h 文件用下面的代码替换里面的内容：

	@interface LibraryAPI: NSObject
	+ (LibraryAPI*)sharedInstance;
	@end

现在打开 LibraryAPI.m 文件，在 @implentation 后面添加如下方法：

	+ (LibraryAPI*)sharedInstance 
	{
	    // 1
	    static LibraryAPI *_sharedInstance = nil;
	
	    // 2 
	    static dispatch_once_t oncePredicate; 
	
	    // 3
	    dispatch_once(&nocePredicate, ^{
	        _sharedInstance = [[LibraryAPI alloc] init];
	    });
	    return _sharedInstance;
	}

在这个短方法中做了这些事情：

* 在这个类中，声明一个静态变量来保存这个实例，保证它是一个全局可用的变量。
* 声明一个静态这是 dispatch_one_t，确保这些初始化代码只能被执行一次。
* 使用 Grand Central Dispatch（GCD）执行一个 block 来初始化 LibraryAPI 实例。这是单例设计模式的关键所在：一个类只能被实例化一次。

接下来执行 sharedInstance，在 dispatch_once block 里的代码是不会被执行的（当它已经被执行过一次后），它会返回之前创建的 LibraryAPI 实例。

	提示：想了解更多关于 GCD 和使用它，请点击这里的教程 Multithreading and Grand Central Dispatch，如何使用 Blocks 在这里。

你现在有一个单例对象来管理专辑了。下一步就是创建一个类用来保存你的专辑数据了。

用 iOS\Cocoa Touch\Object-C class 在 API 文件夹下创建一个新的类，名字叫 PersistencyManager，子类选择 NSObject。

打开 PersistencyManager.h，在顶部引入面文件：

	#import "Album.h"

然后在 @interface 后面加入下面代码：

	- (NSArray *)getAlbums;
	- (void)addAlbums:(Album*)album atIndex:(int)index;
	- (void)deleteAlbumAtIndex:(int)index;

上面的三个方法都需要跟专辑的数据相结合。

打开 PersistencyManager.m，在 @implementation 上面添加如下代码：

	@interface PersistencyManager () {
	    NSMutableArray *albums;
	}

上面的代码是给类添加了一个扩展，这是另一种给类添加私有方法和私有属性的方法，类外面的成员是看不到这些的。这里，你声明了一个 NSMutableArray 来保存专辑的数据。这是一个可变数组，你可以很容易的添加和删除专辑。

现在在 @implementation 下面添加实现代码：

	- (id)init {
	    self = [super init];
	    if (self) {
	        albums = [NSMutableArray arrayWithArray:@[[[Album alloc] initWithTitle:@"Best of Bowie" artist:@"David Bowie" coverUrl:@"http://www.coversproject.com/static/thumbs/album/album_david%20bowie_best%20of%20bowie.png" year:@"1992"],
	        [[Album alloc] initWithTitle:@"It's My Life" artist:@"No Doubt" coverUrl:@"http://www.coversproject.com/static/thumbs/album/album_no%20doubt_its%20my%20life%20%20bathwater.png" year:@"2003"],
	                [[Album alloc] initWithTitle:@"Nothing Like The Sun" artist:@"Sting" coverUrl:@"http://www.coversproject.com/static/thumbs/album/album_sting_nothing%20like%20the%20sun.png" year:@"1999"],
	            [[Album alloc] initWithTitle:@"Staring at the Sun" artist:@"U2" coverUrl:@"http://www.coversproject.com/static/thumbs/album/album_u2_staring%20at%20the%20sun.png" year:@"2000"],
	                [[Album alloc] initWithTitle:@"American Pie" artist:@"Madonna" coverUrl:@"http://www.coversproject.com/static/thumbs/album/album_madonna_american%20pie.png" year:@"2000"]]];
	    }
	    return self;
	}

在 init 里你在数组中加入了 5 张专辑。如果上面的专辑你不喜欢，你可以随意替换成你喜欢的。:]

现存在 PersistencyManager.m 添加下面三个方法：

	- (NSArray*)getAlbums
	{
	        return albums;
	}
	
	- (void)addAlbum:(Album*)album atIndex:(int)index
	{
	        if (albums.count >= index)
	            [albums insertObject:album atIndex:index];
	        else
	        [albums addObject:album];
	}
	
	- (void)deleteAlbumAtIndex:(int)index
	{
	        [albums removeObjectAtIndex:index];
	}

这些方法是获取，添加，删除专辑。

Build 你的项目，确保所有的代码都能正确编译。

现在，你也许想知道为什么会有一个 PersistencyManager 类，因为它都不是单例类。不要着急，接下来第二步，外观设计模式(Facade design patten) 将会探讨 LibraryAPI 和 PersistencyManager 之间的关系。

**设计模式系列文章**：

[iOS 设计模式系列：开篇](http://www.superqq.com/blog/2015/06/10/ios-she-ji-mo-shi-xi-lie-:kai-pian/)

[iOS 设计模式系列：MVC – 设计模式中的国王](http://www.superqq.com/blog/2015/06/11/ios-she-ji-mo-shi-xi-lie-:mvc-she-ji-mo-shi-zhong-de-guo-wang/)

[iOS 设计模式系列：Singleton – 单例模式](http://www.superqq.com/blog/2015/06/13/ios-she-ji-mo-shi-xi-lie-:singleton-dan-li-mo-shi/)

[iOS 设计模式系列：Facade – 外观模式](http://www.superqq.com/blog/2015/06/15/ios-she-ji-mo-shi-xi-lie-:facade-wai-guan-mo-shi/)


