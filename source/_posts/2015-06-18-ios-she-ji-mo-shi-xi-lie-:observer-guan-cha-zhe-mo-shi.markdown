---
layout: post
title: "iOS 设计模式系列：Observer – 观察者模式"
date: 2015-06-18 10:14:51 +0800
comments: true
categories: iOS开发
keywords: 观察者模式, iOS观察者模式, iOS Observer,  设计模式, 刚刚在线
description: 在观察者模式中，当状态发生改变的时候，一个对象会通知另一个对象。这个对象不需要知道另一个对象发生了什么改变─因此非常鼓励这种分离式的设计。这种模式经常用于，当一个属性发生改变时通知跟它相关的对象。

---
在**观察者模式**中，当状态发生改变的时候，一个对象会通知另一个对象。这个对象不需要知道另一个对象发生了什么改变─因此非常鼓励这种分离式的设计。这种模式经常用于，当一个属性发生改变时通知跟它相关的对象。

它通常需要一个观察者(observer)注册跟踪另外一个对象的状态。当状态发生改变的时候，所有的观察对象都会被通知改变。苹果的推送通知服务就是一个这样的例子。

如果你想要一直使用 [MVC 模式](http://www.superqq.com/blog/2015/06/11/ios-she-ji-mo-shi-xi-lie-:mvc-she-ji-mo-shi-zhong-de-guo-wang/)（你确实需要），你如果想在模型和视图之间，不直接相互引用的情况下还要有通信。这时候就要用到观察者模式了。

Cocoa 有两个常用的方法来执行观察者模式：Notifications 和 Key-Value Observing (KVO)。

###通知 Notifications

不要把它和推送、本地通知弄混淆了，通知是基于一个对象（信息发布者）发信息给另一个对象（订阅/监听）的订阅-发布模式的。信息发布者不需要知道任何关于订阅者的信息。

苹果大量的使用了通知。例子，当键盘打开/关闭的时候，系统会分别发送一个 UIKeyboardWillShowNotification/UIKeyboardWillHideNotification。当你的程序要退出的时候，系统会发送一个 UIApplicationDidEnterBackgroundNotification 通知。

	提示：打开 UIApplication.h，在文件最后你会看见系统能发送的通知列表有 20 个之多。

###如何使用通知 Notifications

打开 AlbumView.m 文件，在 initWithFrame:albumCover: 里面的 [self addSubview:indicator] 后面插入下面代码：

	[[NSNotificationCenter defaultCenter] postNotificationName:@“BLDownloadImageNotification” object:self userInfo:@{@“imageView”:coverImage, @“coverUrl”:albumCover}];

这行代码是通过 NSNotificationCenter 单例来发送一个通知。通知内容包括 coverImage 图片视图 和下载封面图片的 URL。这是执行下载专辑封面所需的所有信息。

在 LibraryAPI.m 的 init 方法里，isOnline = NO; 后面添加下面代码：

	[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(downImage:) name:@“BLDownloadImageNotification” object:nil];

这是等式另一边的观察者对象。任何时候只要 AlbumView 类发送一个 BLDownloadImageNotification 通知，系统就会通知 LibraryAPI，因为 LibraryAPI 已经注册了一个相同的观察者通知。qLibraryAPI 执行 downloadImage:。

不管任何时候，在你执行 downloadImage: 之前，一定要记住在你释放这个类的时候一定要取消这个通知的订阅。如果你不正确的取消一个通知的订阅，这个通知可能发送一个已经释放的实例。这会造成你的程序崩溃。

在 LibraryAPI.m 里添加下面代码：

	- (void)dealloc {
	    [[NSNotificationCenter defaultCenter] removerObserver:self];
	}

当这个类释放后，移除一个观察者自己已经注册的所有通知。

这里还有一件事情要做。它可以把下载过的封面图片存在本地，这样 app 就不用一次又一次的下载同一个图片了。

打开 PersistencyManger.h 添加下面两个方法：

	- (void)saveImage:(UIImage*)image filename:(NSString*)filename;
	- (UIImage*)getImage:(NSString*)filename;

在 PersistencyManger.m 中实现：

	- (void)saveImage:(UIImage*)image filename:(NSString*)filename
	{
	    filename = [NSHomeDirectory() stringByAppendingFormat:@“/Documents/%@”, filename];
	    NSData *data = UIImagePNGRepresentation(image);
	    [data writeToFile:filename atomically:YES];
	}
	
	- (UIImage*)getImage:(NSString*)filename 
	{
	    filename = [NSHomeDirectory() stringByAppendingFormat:@“/Documents/%@“, filename];
	    NSData *data = [NSData dataWithContentsOfFile:filename];
	    return [UIImage imageWithData:data];
	}

这里的代码很简单。下载完成的图片会被存储进 Documents 文件夹中，如果没有跟 filename 相匹配的文件，getImage: 会返回 nil。

现在在 LibraryAPI.m 里面添加下面方法：

	- (void)downImage:(NSNotification*)notification
	{
	    // 1 
	    UIImageView *imageView = notification.userInfo[@“imageView”];
	    NSString *coverUrl   = notification.userInfo[@“coverUrl”];
	
	    // 2
	    imageView.image = [persistencyManger getImage:[coverUrl lastPathComponent];
	
	    if (imageView.image == nil) {
	        // 3 
	        dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
	            UIImage *image = [httpClient downloadImage:coverUrl];
	            dispatch_sync(dispatch_get_main_queue(), ^{
	                imageView.image = image;
	                [persistencyManger saveImage:image filename:[coverUrl lastPathComponent];
	            });
	        });
	    }
	}

现在来分析上面的代码：

1. 通知执行 downloadImage 方法，方法接受这个这个通知对象，就像它是一个变量一样。通知里会传递 UIImageView 和图片的 URL。
2. 如果以前已经下载过，就从 PersistencyManger 里取出图片。
3. 如果图片没有下载过，使用 HTTPClient 开始下载图片。
4. 当下载完成的时候，在图片视图中显示图片，用 PersistencyManger 存储它到本地。

此外，你用外观模式(Facade pattern)隐藏了另外一个下载图片的复杂类。通知发送者不关心图片的来源，不管是从网上下载的还是本地存储的。

构建和运行你的 app，可以看到漂亮的封面已经出现在 HorizontalScroller 里面了：

![1](http://cdn5.raywenderlich.com/wp-content/uploads/2013/08/design-patterns-4-297x320.png)

停止你的 app，然后再运行它。注意这里显示封面已经没有延时了，因为它们已经下载到本地了。你可断开网络试试，你的 app 同样可以完美的运行。虽然已经很完美了，但是仍然有一个小问题：就是下载提示的小菊花仍然一直在转动，从来没有停止过！哪里出问题了？

当你开始下载图片的时候，你运行了下载提示符，但当图片下载完成的时候，你没有停止下载提示符的方法。你也可以当每个图片下载完成的时候再发送一个通知，当然另一个代替方案，你可以使用另一种观察者模式，KVO。

###键 – 值 观察 (Key-Value Observing KVO)

在 [KVO](http://www.superqq.com/blog/2015/06/05/ios-kvogai-shu-yu-shi-jian/)  里，一个对象的任何一个特别的属性改变后都可以请求一个通知；不管是它自己的还是其它的对象。如果你感兴趣，你可以在这里读到更多的信息：Apple’s KVO Programming Guide.

###如何使用 KVO 模式

如上所述，[KVO](http://www.superqq.com/blog/2015/06/05/ios-kvogai-shu-yu-shi-jian/) 的原理是允许一个对象观察一个属性的改变。你所要关心的是，使用 KVO 观察 UIImageView 的 image 属性是否已经改变，就是它是否已经存储了图片。

打开 AlbumView.m，在 initWithFrame:albumCover: 里的 [self addSubview:indicator]; 后面添加下面代码：

	[coverImage addObserver:self forKeyPath:@“image” options:0 context:nil];

不添加在自己上面(self)，在当前的类里，观察 coverImage 的 image 属性。

当你使用过后，你同样需要注销这个观察(observer)。继续在当前文件里面添加下面代码：

	- (void)dealloc
	{
	    [coverImage removerObserver:self forKeyPath:@“image”];
	}

最后，添加下面方法：

	- (void)observerValueForKeyPath:(NSString*)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context
	{
	    if ([KeyPath isEqualToString:@“image”])
	    {
	        [indicator stopAnimating];
	    }
	}

你必须在每个类里执行这个方法。如果观察的属性改变了，系统每次都会执行这个方法。上面的代码里，当 image 的属性发生改变时，你停止下载提示符的运行。这种方法，当图片下载完成，下载提示转动的小菊花将会停止转动。

构建个运行你的项目。你会看到小菊花已经不在了：

![2](http://cdn3.raywenderlich.com/wp-content/uploads/2013/08/design-pattern-stage3-287x320.png)

	提示：永远记住，当你释放内存，你一定要移除这些观察(observers)，或者是当你的程序发送这些不存的观察对象时会造成程序崩溃。

如果你玩弄一会你的 app，然后关闭它，你程序的当前状态并没有被存储下来。当程序启动的时候你看到的视图并不是上次退出时的样子。

为了更正这些，你需要使用下一项设计模式：备忘录 (Memento)。

**设计模式系列文章**：

[iOS 设计模式系列：开篇](http://www.superqq.com/blog/2015/06/10/ios-she-ji-mo-shi-xi-lie-:kai-pian/)

[iOS 设计模式系列：MVC – 设计模式中的国王](http://www.superqq.com/blog/2015/06/11/ios-she-ji-mo-shi-xi-lie-:mvc-she-ji-mo-shi-zhong-de-guo-wang/)

[iOS 设计模式系列：Singleton – 单例模式](http://www.superqq.com/blog/2015/06/13/ios-she-ji-mo-shi-xi-lie-:singleton-dan-li-mo-shi/)

[iOS 设计模式系列：Facade – 外观模式](http://www.superqq.com/blog/2015/06/15/ios-she-ji-mo-shi-xi-lie-:facade-wai-guan-mo-shi/)

[iOS 设计模式系列：Decorator – 装饰器模式](http://www.superqq.com/blog/2015/06/16/ios-she-ji-mo-shi-xi-lie-:decorator-zhuang-shi-qi-mo-shi/)

[iOS 设计模式系列：Adapter – 适配器模式](http://www.superqq.com/blog/2015/06/17/ios-she-ji-mo-shi-xi-lie-:adapter-gua-pei-qi-mo-shi/)

[iOS 设计模式系列：Memento – 备忘录模式](http://www.superqq.com/blog/2015/06/19/ios-she-ji-mo-shi-xi-lie-:memento-bei-wang-lu-mo-shi/)