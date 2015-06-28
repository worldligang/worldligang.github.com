---
layout: post
title: "iOS 设计模式系列：Archiving – 归档模式"
date: 2015-06-20 06:54:13 +0800
comments: true
categories: iOS开发
keywords: 归档模式, iOS归档模式, iOS Archiving,  设计模式, 刚刚在线
description: 归档模式在新代码里，如果文件存在，NSKeyedUnarchiver 就从文件加载专辑数据。如果不存在，它就创建专辑数据，然后立即存储，为下一次启动 app 使用。

---

**备忘录模式**就是苹果中一种归档形式。它能够把对象转化成一种数据流，在不依赖于外部类的私有属性的情况下来存储和取出数据。你可以在 iOS 6 by Tutorials 这本书的第 16 章读到更多的相关信息。或者这里 Apple’s Archives and Serializations Programming Guide。

###如何使用归档 Archiving

首先，你需要声明 Album，用 NSCoding 协议来归档数据。打开 Album.h 文件，修改 @ interface 行如下：
	
	@interface Album : NSObject <NSCoding>

在 Album.m 添加下面两个方法：

	- (void)encodeWithCode:(NSCode *)aCoder
	{
	    [aCode encodeObject:self.year forKey:@“year”];
	    [aCode encodeObject:self.title forKey:@“album”];
	    [aCode encodeObject:self.artist forKey:@“artist”];
	    [aCode encodeObject:self.coverUrl forKey:@“cover_url”];     [aCode encodeObject:self.genre forKey:@“genre”];
	}
	
	- (id)initWithCode:(NSCode *)aDecoder
	{
	    self = [super init];
	    if (self) {
	        _year = [aDecoder decodeObjectForKey:@“year”];
	        _title = [aDecoder decodeObjectForKey:@“album”];
	        _artist = [aDecoder decodeObjectForKey:@“artist”];
	        _coverUrl = [aDecoder decodeObjectForKey:@“cover_url”];
	        _genre = [aDecoder decodeObjectForKey:@“genre”];
	    }
	    return self;
	}

当你存档这个类的实例时调用 encodeWithCoder:。相反，当你通过解档来创建一个 Album 实例时，需要调用 iniWithCode: 。就这么简单，非常强大。

现在 Album 类可以存档了，添加的代码实际上是存储和加载专辑列表的。

在 PersistencyManger.h 里添加下面方法：

	- (void)saveAlbums;

当存储专辑的时候调用些方法。

现在，在 PersistencyManger.m 里添加方法实现：

	- (void)saveAlbums
	{
	    NSString *filename = [NSHomeDirectory() stringByAppendingString:@“/Document/albums.bin”];
	    NSData *data = [NSKeyedArchiver archivedDataWithRootObject:albums];
	    [data writeToFile:filename atomically:YES];
	}

NSKeyedArchiver 把专辑数组存档进一个叫 album.bin 的文件里。

当你存档的一个对象包含另一个对象的时候，存档自动递归的把子对象、子对象的子对象一并存储。在这个实例里，存储的是一个专辑实例的数组。因为数组和 Album 都支持 NSCopying 的接口，数组里的东西都会被自动存档。

现在用下面代码替换 PersistencyManger.m 里的 init 方法：

	- (id)init
	{
	    self = [super init];
	    if (self) {
	        NSData *data = [NSData dataWithContentsOfFile:[NSHomeDirectory() stringByAppendString:@“/Document/albums.bin”]];
	        albums = [NSKeyedUnarchiver unarchiveObjectWithData:data];
	        if (albums == nil) {
	            albums = [NSMutableArray arrayWithArray:
	                 @[[[Album alloc] initWithTitle:@"Best of Bowie" artist:@"David Bowie" coverUrl:@"http://www.coversproject.com/static/thumbs/album/album_david%20bowie_best%20of%20bowie.png" year:@"1992"],
	                 [[Album alloc] initWithTitle:@"It's My Life" artist:@"No Doubt" coverUrl:@"http://www.coversproject.com/static/thumbs/album/album_no%20doubt_its%20my%20life%20%20bathwater.png" year:@"2003"],
	                 [[Album alloc] initWithTitle:@"Nothing Like The Sun" artist:@"Sting" coverUrl:@"http://www.coversproject.com/static/thumbs/album/album_sting_nothing%20like%20the%20sun.png" year:@"1999"],
	                 [[Album alloc] initWithTitle:@"Staring at the Sun" artist:@"U2" coverUrl:@"http://www.coversproject.com/static/thumbs/album/album_u2_staring%20at%20the%20sun.png" year:@"2000"],
	                 [[Album alloc] initWithTitle:@"American Pie" artist:@"Madonna" coverUrl:@"http://www.coversproject.com/static/thumbs/album/album_madonna_american%20pie.png" year:@"2000"]]];
	                [self saveAlbums];
	        }
	    }
	    return self;
	}

在新代码里，如果文件存在，NSKeyedUnarchiver 就从文件加载专辑数据。如果不存在，它就创建专辑数据，然后立即存储，为下一次启动 app 使用。

当然你也可以在 app 后台运行的时候再存储专辑数据。现在它看起来不是很有必要，但如果你在稍后添加修改专辑数据呢？你需要确保所有的数据修改都会被存储。

在 LibraryAPI.h 添加下面的方法

	- (void)saveAlbums;

因为程序的所以有服务都需要通过 LibraryAPI 提供，这就是程序为什么需要 PersistenrcyManger 来存储专辑数据。

现在在 LibraryAPI.m 中添加实现方法：

	- (void)saveAlbums
	{
	    [persistencyManger saveAlbums];
	}

这里是从 LibraryAPI 调用 PersistencyManger 来存储专辑。

在 ViewController.m 文件 saveCurrentState 的后面添加下面代码：

[[LibraryAPI sharedInstance] saveAlbums];

上面的代码是每当 ViewController 存储当前状态时，使用 LibraryAPI 来存储专辑数据。

构建你的 app，检查所有的都能正确编译。

不幸的是，没有一种简单的方法来验证数据是否被正确的创建。你可以在 Finder 里检查模拟器下当前 app 的 Documents 文件夹，来看看专辑数据文件是否被创建，但是为了查看其它变化你还需要添加一些数据来改变专辑数据。

但是，为了修改数据，假使为了删除专辑而需要临时添加一个删除选项，但这个选项并不会一直在你的库中存在？此外，由于误操作，错误的删除了一个专辑，但如果给这个操作添加一个撤消选项不是很好吗？

现在是时候讨论最后一个设计模式了：[命令 Command](http://www.superqq.com/blog/2015/06/21/ios-she-ji-mo-shi-xi-lie-:command-ming-ling-mo-shi/)。

**设计模式系列文章**：

[iOS 设计模式系列：开篇](http://www.superqq.com/blog/2015/06/10/ios-she-ji-mo-shi-xi-lie-:kai-pian/)

[iOS 设计模式系列：MVC – 设计模式中的国王](http://www.superqq.com/blog/2015/06/11/ios-she-ji-mo-shi-xi-lie-:mvc-she-ji-mo-shi-zhong-de-guo-wang/)

[iOS 设计模式系列：Singleton – 单例模式](http://www.superqq.com/blog/2015/06/13/ios-she-ji-mo-shi-xi-lie-:singleton-dan-li-mo-shi/)

[iOS 设计模式系列：Facade – 外观模式](http://www.superqq.com/blog/2015/06/15/ios-she-ji-mo-shi-xi-lie-:facade-wai-guan-mo-shi/)

[iOS 设计模式系列：Decorator – 装饰器模式](http://www.superqq.com/blog/2015/06/16/ios-she-ji-mo-shi-xi-lie-:decorator-zhuang-shi-qi-mo-shi/)

[iOS 设计模式系列：Adapter – 适配器模式](http://www.superqq.com/blog/2015/06/17/ios-she-ji-mo-shi-xi-lie-:adapter-gua-pei-qi-mo-shi/)

[iOS 设计模式系列：Observer – 观察者模式](http://www.superqq.com/blog/2015/06/18/ios-she-ji-mo-shi-xi-lie-:observer-guan-cha-zhe-mo-shi/)

[iOS 设计模式系列：Memento – 备忘录模式](http://www.superqq.com/blog/2015/06/19/ios-she-ji-mo-shi-xi-lie-:memento-bei-wang-lu-mo-shi/)

[iOS 设计模式系列：Archiving – 归档模式](http://www.superqq.com/blog/2015/06/20/ios-she-ji-mo-shi-xi-lie-:archiving-gui-dang-mo-shi/)