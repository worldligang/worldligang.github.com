---
layout: post
title: "iOS 设计模式系列：Memento – 备忘录模式"
date: 2015-06-19 06:31:19 +0800
comments: true
categories: iOS开发
keywords: 备忘录模式, iOS备忘录模式, iOS Memento,  设计模式, 刚刚在线
description: 备忘录模式是捕获和具体化当前对象的内部状态。换句话说，就是在某个地方存储个东西。稍后，在不违反封装的情况下能够恢复这个具体化的状态；就是说，一些私有数据被私下存储下来了。

---

**备忘录模式**是捕获和具体化当前对象的内部状态。换句话说，就是在某个地方存储个东西。稍后，在不违反封装的情况下能够恢复这个具体化的状态；就是说，一些私有数据被私下存储下来了。

###如何使用备忘录模式

添加下面两个方法到 ViewController.m 文件：

	- (void)saveCurrentState
	{
	    // 当用户退出应用之后再重新打开，他想要跟他之前退出时一样的状态
	    // 退出应用，这个时候我们需要做的是把当前显示的专辑存储下来
	    // 因为只有一小片信息，我们可用 NSUserDefaults 来存储信息
	    [[NSUserDefaults standardUserDefaults] setInteger:currentAlbumIndex forKey:@“currentAlbumIndex”];
	}
	
	- (void)loadPreviousState
	{
	    currentAlbumIndex = [[NSUserDefaults standardUserDefaults] integerForKey@“currentAlbumIndex”];
	    [self showDataForAlbumAtIndex:currentAlbumIndex];
	}

saveCurrentState 存储当前专辑的索引到 NSUserDefaults ─ NSUserDefaults 是一个标准数据存储，iOS 用来专门存放程序设置和数据。

loadPreviousState 加载这之前存储的专辑索引。这不是备忘录模式的全部，不过你已经达到目的了。

现在，在 ViewController.m 里，滚动视图初始化之前，在 viewDidLoad 里添加下面一行：

	[self loadPreviousState];

当程序启动的时候加载上一次存储的状态。但是你在哪里存储程序的当前状态呢？你需要使用通知来做这样的事情。当程序进入后台时，iOS 会发送一个 UIApplicationDidEnterBackgroundNotification 通知。你可利用这个通知调用 saveCurrentState。就这么方便？

在 viewDidLoad: 最后面添加下面一行

	[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(saveCurrentState) name:UIApplicationDidEnterBackgroundNotification object:nil];

现在，当你的 app 进入后台运行后，ViewController 会自动调用 saveCurrentState 存储当前的状态。

现在，添加下面代码：

	- (void)dealloc
	{
	    [[NSNotificationCenter defaultCenter] removeObserver:self];
	}

这里是确保当 ViewController 被释放时，移除类的 [Observer](http://www.superqq.com/blog/2015/06/05/ios-kvogai-shu-yu-shi-jian/)。

构建和运行你的 app，点击到一个专辑，用 Command+Shift+H（如果你使用的是模拟器的话） 将程序在后台运行，然后关掉 app。重启 app，检查之前选择的专辑是不是居中显示：

![1](http://cdn3.raywenderlich.com/wp-content/uploads/2013/09/2013-09-02_10-51-27-214x320.png)

专辑数据看起来是对的，但是正确的专辑封面确没有居中，哪出问题了？

这就是可选方法 initialViewIndexForHorizontalScroller 的用处！因为这个方法没有被委托执行，ViewController 在这种情况下总是会显示默认的第一个专辑封面。

修复这个问题，在 ViewController.m 中添加如下代码：

	- (NSInteger)initialViewIndexForHorizontalScroller:(HorizontalScroller *)scroller
	{
	    return currentAlbumIndex;
	}

现在 HorizontalScroller 的第一个视图总是会被设置成 currentAlbumIndex 索引的图片。这种方法能够确保你的 app 有一个非常棒的用户体验，并且它是可恢复的。

重新运行你的 app，滚动专辑封面，关闭 app，然后重启确保问题已经得到解决：

![2](http://cdn5.raywenderlich.com/wp-content/uploads/2013/09/2013-09-02_10-56-59-214x320.png)

如果你查看 PersistencyManger 的初始化方法，你会注意到专辑的数据是一种硬编码，PersistencyManger 每次创建，数据也会重复创建一次。有没有一种更好的方法当专辑列表被创建的时候就存储它们呢。那么如何把专辑数据存储到文件里呢？

一种选择就是循环访问 Album 的属性，然后把它存储在一个 plist 文件里，当需要它们的时候重新创建一个 Album 的实例。这不是最好的选择，这需要你在每一个类里根据不同的数据或属性写特定的代码。例子，如果稍后你需要一个电影的类，里面有一些不同的属性，存储和加载这些数据你就需要写一些新的代码。

此外，你不能在每一个类的实例里存储私有变量，因为他们是不可访问的外部类。这就是为什么苹果要创建归档 (Archiving) 机制。


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