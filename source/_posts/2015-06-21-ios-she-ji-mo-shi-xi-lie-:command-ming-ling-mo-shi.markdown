---
layout: post
title: "iOS 设计模式系列：Command – 命令模式"
date: 2015-06-21 23:39:03 +0800
comments: true
categories: iOS开发
keywords: 命令模式, iOS命令模式, iOS Command,  设计模式, 刚刚在线
description: 命令模式封装一个请求或行为作为一个对象。封装的请求比原的更加灵活，可以在对象之间传递，储存，动态修改，或放入一个队列。苹果的Target-Action调用机制已经实现了命令模式。


---

**命令模式**封装一个请求或行为作为一个对象。封装的请求比原的更加灵活，可以在对象之间传递，储存，动态修改，或放入一个队列。苹果的Target-Action调用机制已经实现了命令模式。

你可以查看跟多关于[Target-Action](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaEncyclopedia/Target-Action/Target-Action.html)的苹果官方文档，
NSInvocation包含一个target对象，一个方法和一些参数。这个对象可以按需要动态修改。这是一个非常好的命令模式的列子。减少发送对象和接受对象之间的操作，直接写成一个请求或请求链。

###如何用命令模式


调用之前，你需要设置取消动作的框架。所以你需要定义一个UIToolBar和NSMutableArray保存需要撤销的堆栈。

添加代码到ViewController.m 文件：

	    UIToolbar *toolbar;
    // We will use this array as a stack to push and pop operation for the undo option
	    NSMutableArray *undoStack;
	    
	    
创建一个toolbar将显示新的动作按钮，以及一个数组作为命令队列。

将下面的代码添加到viewDidLoad：

	  toolbar = [[UIToolbar alloc] init];
    UIBarButtonItem *undoItem = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemUndo target:self action:@selector(undoAction)];
    undoItem.enabled = NO;
    UIBarButtonItem *space = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemFlexibleSpace target:nil action:nil];
    UIBarButtonItem *delete = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemTrash target:self action:@selector(deleteAlbum)];
    [toolbar setItems:@[undoItem,space,delete]];
    [self.view addSubview:toolbar];
    undoStack = [[NSMutableArray alloc] init];

上面的代码创建了一个toolbar和两个按钮，它还创建一个空的撤消堆栈。这里的撤销按钮被禁用因为撤销栈为空。

将下面代码添加到ViewController.m：

	- (void)viewWillLayoutSubviews
	{
	    toolbar.frame = CGRectMake(0, self.view.frame.size.height-44, self.view.frame.size.width, 44);
	    dataTable.frame = CGRectMake(0, 130, self.view.frame.size.width, self.view.frame.size.height - 200);
	}

在ViewController.m增加三个方法：增加、删除、撤销。

增加一个新专辑代码如下：


	- (void)addAlbum:(Album*)album atIndex:(int)index
	{
	    [[LibraryAPI sharedInstance] addAlbum:album atIndex:index];
	    currentAlbumIndex = index;
	    [self reloadScroller];
	}

在这里你添加相册，将其设置为当前专辑索引，并重新加载。

接下来就是删除方法：

	- (void)deleteAlbum
	{
	    // 1
	    Album *deletedAlbum = allAlbums[currentAlbumIndex];
	 
	    // 2
	    NSMethodSignature *sig = [self methodSignatureForSelector:@selector(addAlbum:atIndex:)];
	    NSInvocation *undoAction = [NSInvocation invocationWithMethodSignature:sig];
	    [undoAction setTarget:self];
	    [undoAction setSelector:@selector(addAlbum:atIndex:)];
	    [undoAction setArgument:&deletedAlbum atIndex:2];
	    [undoAction setArgument:&currentAlbumIndex atIndex:3];
	    [undoAction retainArguments];
	 
	    // 3
	    [undoStack addObject:undoAction];
	 
	    // 4
	    [[LibraryAPI sharedInstance] deleteAlbumAtIndex:currentAlbumIndex];
	    [self reloadScroller];
	 
	    // 5
	    [toolbar.items[0] setEnabled:YES];
	}

这是一段新的令人激动的代码，讲解如下：

1. 得到要删除的专辑信息
2. Define an object of type NSMethodSignature to create the NSInvocation, which will be used to reverse the delete action if the user later decides to undo a deletion. The NSInvocation needs to know three things: The selector (what message to send), the target (who to send the message to) and the arguments of the message. In this example the message sent is delete’s opposite since when you undo a deletion, you need to add back the deleted album.
3. After the undoAction has been created you add it to the undoStack. This action will be added to the end of the array, just as in a normal stack.
4. Use LibraryAPI to delete the album from the data structure and reload the scroller.
5. Since there’s an action in the undo stack, you need to enable the undo button.

	
		提示: With NSInvocation, you need to keep the following points in mind:
	
	    The arguments must be passed by pointer.
	    The arguments start at index 2; indices 0 and 1 are reserved for the target and the selector.
	    If there’s a chance that the arguments will be deallocated, then you should call retainArguments.
	    
最后，添加撤销方法：

	- (void)undoAction
	{
	    if (undoStack.count > 0)
	    {
	        NSInvocation *undoAction = [undoStack lastObject];
	        [undoStack removeLastObject];
	        [undoAction invoke];
	    }
	 
	    if (undoStack.count == 0)
	    {
	        [toolbar.items[0] setEnabled:NO];
	    }
	}

这个撤销操作是取消栈里最后一个对象，This object is always of type NSInvocation and can be invoked by calling … invoke. This invokes the command you created earlier when the album was deleted, and adds the deleted album back to the album list. Since you also deleted the last object in the stack when you “popped” it, you now check to see if the stack is empty. If it is, that means there are no more actions to undo. So you disable the Undo button.

运行你的app，测试撤销功能，删除一个专辑（或两个）然后点击撤销按钮看看效果：

![1](http://cdn1.raywenderlich.com/wp-content/uploads/2013/08/design-pattern-stage4.png)

这也是一个很好的地方来测试是否你的相册数据保留会变化。现在，如果你删除一个专辑，app退到后台，并终止应用程序，在下次启动应用程序显示的专辑列表不会有删除专辑信息。

这里的源代码完成的项目：[最后bluelibrary](cdn2.raywenderlich.com/wp-content/uploads/2013/08/BlueLibrary-final.zip)

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