---
layout: post
title: "iOS 设计模式系列：MVC – 设计模式中的国王"
date: 2015-06-11 13:41:20 +0800
comments: true
categories: iOS开发
keywords: 设计模式, MVC, 单例模式, 外观模式, 装饰器模式, 适配器模式, 观察者模式,备忘录模式,归档模式,命令模式, cocoapods, 个人博客, 刚刚在线
description: "iOS 设计模式系列：MVC、Singleton – 单例模式、Facade – 外观模式、Decorator – 装饰器模式、Adapter – 适配器模式、Observer – 观察者模式、Memento – 备忘录模式、Archiving – 归档模式、Command – 命令模式"

---

![icon](http://cdn5.raywenderlich.com/wp-content/uploads/2013/07/mvcking.png)

模型(Model) 视图(View) 控制器(Controller) ([MVC](http://www.superqq.com/blog/2014/11/10/qian-xi-mvche-shuo-mei-de-guo-cheng/)) 是 Cocoa 中的一种行为模块，并且也是所有 Cocoa 设计模式中使用最多的。在程序中按照它们的角色来分类这些对象并且鼓励你的基础代码也按照这个来划分。

这三种角色是：

* 模型：这是一种暂时保存你程序数据和定义如何操作它的对象。一个例子，在你的程序中 Album 类就是一个模型。
* 视图：这种对象是控制模型里的数据如何显示出来的，用户也可操作这些对象；事实上，包含所有的 UIView 和他们的子类。在你的程序中视图是 AlbumView 类。
* 控制器：控制器是一个中介者协调所有的工作。他使数据从模型传出来然后显示在视图上，监听事件，在必要的时候操作数据。你能猜到哪个类你的控制器吗？对了，它就是 ViewController。

在你的程序中一个好的设计模式的实现意味者所有的对象都在这些群组中。

下图很好的描述了视图和模型之间的通信过程：

![icon](http://cdn2.raywenderlich.com/wp-content/uploads/2013/07/mvc0.png)


当有数据发生改变的时候模型通知控制器，然后控制器更新视图数据。 反过来，当用户在视图执行一些操作后，视图会通知控制器，如果有需要或是要取回请求数据，控制器也会更新模型或者。

你可能想我为什么不能丢掉控制器，在同一个类里实现视图和模型，这样看起来更简单。

归根结底，这是为是分离代码和提高代码的可重用性。理想状态下视图应该完全从模型中分离出来。如果视图不依赖一个特殊的模型来实现，那么另外一个模型就可以重复利用这个视图显示一些其它不同的数据。

举一个例子，在将来某一天你想在你的音乐库中加入一些电影或者一些书，你可以继续使用原来的 AlbumView 视图来显示这个电影或者图书对象。此外，如果你想创建一个新项目，这个项目中的一些东西跟音乐专辑有关，你可以很简单的拿来重用 Album 类，因为它不依赖于其它任何视图。这就是 MVC 的精髓所在。

###如何使用 MVC 模式

首先，你要保证在你的项目中任何一个类都有一个控制器，一个模型，一个视图，一个类中的函数不能有两种作用。到目前为至，你已经完成了一项很棒的工作，创建了一个 Album 类和一个 AlbumView 类。

第二步，根据这些方法的作用你应该创建三个文件夹，来存放不同类别的代码，每种类型一个文件夹。

在菜单上选择 File\New\Group (或者按 Command+Option+N) ，创建名称为 Model，重复上面的动作，创建 View 和 Controller 文件夹。

现在拖动 Album.h 和 Album.m 文件到 Model 文件夹。拖动 AlbumView.h 和 AlbumView.m 文件到 View 文件夹，最后拖动 ViewController.h 和 ViewController.m 文件到 Controller 文件夹中。

现在，项目文件的结构目录应该是这个样子的：

![icon](http://cdn1.raywenderlich.com/wp-content/uploads/2013/07/mvc2-255x320.png)

如果没有其它的文件漂在外面，你的项目看起来已经非常棒了。显然你还可能有其它的文件夹和类，但是你程序的核心内容应该包含在这三个分类中。

现在你的文件已经组织好了，你需要从其它地方获取专辑数据了。你需要创建一个 API 类用来管理你的代码的数据来源 — 现在是机会开始讨论下一个设计模式了 — 单例模式(The Singleton Pattern)。

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

