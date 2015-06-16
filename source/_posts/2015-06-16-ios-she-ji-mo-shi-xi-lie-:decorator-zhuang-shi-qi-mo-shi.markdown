---
layout: post
title: "iOS 设计模式系列：Decorator – 装饰器模式"
date: 2015-06-16 07:44:51 +0800
comments: true
categories: iOS开发
keywords: 装饰器模式, iOS装饰器模式, iOS Decorator, 刚刚在线
description: "iOS 设计模式系列：MVC、Singleton – 单例模式、Facade – 外观模式、Decorator – 装饰器模式、Adapter – 适配器模式、Observer – 观察者模式、Memento – 备忘录模式、Archiving – 归档模式、Command – 命令模式"

---

**装饰器模式**可以在不修改代码的情况下灵活的为一对象添加行为和职责。当你要修改一个被其它类包含的类的行为时，它可以代替子类化方法。

在 Object-C 里有两个种非常常见的实现模式：分类(Category)和委托(Delegation)。
分类 Category

分类是一种非常强大的机制，它允许你在一个已存在的类里添加新方法，而不需要去为他添加一个子类。新方法在编译的时候添加，它能像这个类的扩展方法一样正常执行。一个装饰器跟类的定义稍微有点不同的就是，因为装饰器不能被实例化，它只是一个扩展。

	提示：除了你自己类的扩展，你还可在任何 Cocoa 类里的扩展添加方法。

###如何使用分类

现在你有一个 Album 对象，你需要把它显示在一个表单视图里(table view)：

![1](http://cdn1.raywenderlich.com/wp-content/uploads/2013/08/design-patterns-category1.png)

专辑的标题从哪里来？Album 只是一个模型对象，它才不会去关心你如果去显示这些数据。为了这些，你需要给 Album 类添加一些额外的代码，但是请不要直接修改这个类。

你现在就需要为 Album 添加一个分类 (category) 的扩展；它将定义一个新地方法用来返回一个数据结构，这个数据结构可以很容易的被 UITableViews 使用。

这个数据结构看起来如下：

![2](http://cdn5.raywenderlich.com/wp-content/uploads/2013/08/delegate2-480x67.png)

为 Album 添加一个分类，导航 File\New\File… 选择 Object-C category 模版─不要习惯的去选择 Object-C class，在 Category 后面输入 TableRepresentation，Category to 后面输入 Album。

	提示：你有没有注意这个新文件的名字？Album+TableRepresentation 说明它是 Album 类的一个扩展。这个习惯很重要，因为第一这很容易读，第二防止你或者其他人创建的分类跟其冲突。

打开 Album+TableRepresentation，加入下面的方法原型：

	- (NSDictionary*)tr_tableRepresentation;

注意，这是一个 tr_ 开头的方法名，就像是这个分类名字的缩写一样：TableRepresentation。其次，这个习惯会避免这个方法跟其它方法重名！

	提示：如果分类 (Category) 声明的一个方法跟原始类的一个方法重名，或者跟同类里的的另一个分类名字重复（或者是它的父类），当它在运行的时候，它就不知道要执行哪个方法。如果是在你自己类的分类里，它不太可能出现大的问题，但是如果一个标准 Cocoa 或者 Cocoa Touch 类里面添加这个分类的方法，就可能会引起严重的问题。

打开 Album+TableRepersentation.m 文件添加下面的方法：

	- (NSDictionary*)tr_TableRepersentation
	{
	    return @{@"titles":@[@"Artist", @"Album", @"Genre", @"Year"],
	            @"values":@[self.artist, self.title, self.genre, self.year]};
	};

考虑一会，为什么这种模式如些强大：

* 你能够直接使用 Album 的属性。
* 你已经添加在 Album 类里，但它并不是它的子类。如果子类需要，你同样也可以这样做。
* 这样一个简单的添加，Album 类的数据返回一个 UITableView 可用的数据结构，但并不需要修改 Album 的代码。

苹果在基础类里大量的使用了分类设计模式。去看看他们是怎么做的，打开 NSString.h。找到 @interface NSString，你将会看到这个类定义了三个分类：NSStringExtensionMethods, NSExtendedStringPropertyListParsing 和 NSStingDeprecated。在代码片里，分类将帮助你保持方法的组织性和分离必。

###委托 Delegation

另外一种装饰器的设计模式是，委托 (Delegation)，它是一种机制，一个对象代表另外一个对象或者其相互合作。例子，当你使用 UITableView 的时候，其中一个方法是你必需要执行的，tableView:numberOfRowsInSection:。

你可能并不期望 UITableView 知道每个 section 中有多少行，这是程序的特性。因此，计算每个 section 有多少行的工作就交给了 UITableView 的委托 (delegate)。它允许 UITableView 类不依赖它显示的数据。

当你创建了一个新的 UITableView 的时候，这里有一个类似的解释：

![3](http://cdn5.raywenderlich.com/wp-content/uploads/2013/08/delegate-480x252.png)

UITableView 对象的工作就是显示一个表单视图。然而，最终它都需要一些它信息，它并不拥有这些信息。然后，它会转向它的委托，发送一个添加信息的消息。在 Object-C 中实现委托模式，一个类可以通过协议 (protocol) 来声明一个可选和必选的方法。稍后，在这个教程你将覆盖一个协议 (protocols)。

它看起来比子类更容易，覆盖需要的方法，但是考虑如果是单类的话你只能创建子类。如果你想一个对象委托两个或者多个对象的时候，子类化的方法是不能实现的。

	提示：这是一个很重要的模式。苹果在 UIKit 类中大量的使用了此方法：UITableView, UITextView, UITextField, UIWebView, UIAlert, UIActionSheet, UICollectionView, UIPickerView, UIGestureRecognizer, UIScrollView。这个列表还可以有很多。

###如何使用委托模式

打开 ViewController.m，在顶部引入如下文件

	#import "LibraryAPI.h"
	#import "Album+TableRepresentation.h"

现在，在类的扩展里的添加一些私有变量，它们看起来如下：

	@interface ViewController (){
	    UITableView *dataTable;
	    NSArray *allAlbums;
	    NSDictionary *currentAlbumData;
	    int currentAlbumIndex;
	}
	@end

现在，替换类扩展里的 @interface 这一行，完成后如下：

	@interface ViewController () <UITableViewDataSoure, UITableViewDelegate> {

这就是如何设置一个正确的委托─把它相象成允许一个委托来履行一个方法的合同。这里，表明 ViewController 将会遵照 UITableViewDataSource 和 UITableViewDelegate 协议。这种方法下 UITableView 必须执行它自己的委托方法。

下面，用下面的代码替换 viewDidLoad:

	- (void)viewDidLoad {
	    [super viewDidLoad];
	    // 1
	    self.view.backgroundColor = [UIColor colorWithRed:0.76f green:0.81f blue:0.87f alpha:1];
	    currentAlbumIndex = 0;
	
	    //2
	    allAlbums = [[LibraryAPI sharedInstance] getAlbums];
	
	    // 3
	    // the uitableview that presents the album data
	    dataTable = [[UITableView alloc] initWithFrame:CGRectMake(0, 120, self.view.frame.size.width, self.view.frame.size.height-120) style:UITableViewStyleGrouped];
	    dataTable.delegate = self;
	    dataTable.dataSource = self;
	    dataTable.backgroundView = nil;
	    [self.view addSubview:dataTable];
	}

这里分析下上面的代码：

1. 把背景色改为漂亮的深蓝色。
2. 从 API 获取一个列表，它包含所有的专辑数据。不能直接使用 PersistencyManager。
3. 创建一个 UITableView。你声明了视图控制器是 UITableView delegate/data source；因此，UITableView 将会提供视图控制器需要的所有信息。

现在，在 ViewController.m 里面添加如下方法：

	- (void)showDataForAlbumAtIndex:(int)albumIndex{
	    // defensive code: make sure the requested index is lower than the amount of albums
	    if (albumIndex < allAlbums.count) {
	        // fetch the album
	        Album *album = allAlbums[albumIndex];
	        // save the albums data to present it later in the tableview
	        currentAlbumData = [album tr_tableRepresentation];
	    } else {
	        currentAlbumData = nil;
	    }
	
	    // we have the data we need, let's refresh our tableview
	    [dataTable reloaddata];
	}

showDataForAlbumAtIndex: 从专辑数组中取出需要的专辑数据。当你需要显示新数据的时候，你只需要重载数据 (relaodData)。这是因为 UITableView 需要请求它的委托代理，像有多少 sections 将会在表单视图中显示，每个 section 中有多少行，每行看起来是什么样的。

在 viewDidLoad 中添加下面代码

	[self showDataForAlbumAtIndex:currentAlbumIndex];

当程序运行的时候它会加载当前的专辑信息。由于 currentAlbumIndex 的预设值为 0，所以会显示收藏中的第一张专辑信息。

构建并运行你的项目，你的程序会崩溃掉，在控制台会输入如下的异常：

![4](http://cdn5.raywenderlich.com/wp-content/uploads/2013/09/2013-09-01_15-57-32-700x112.png)

出现什么问题了？你已经声明了 ViewController 中的 UItableView 的委托(delegate)和数据源(data source)。但是在这种情况下，你必需执行所有的必需方法─包含 tableView:numberOfRowsInsection:─你现在还没有它。

在 ViewContrller.m 的 @implementation 和 @end 的任何地方添加如下代码：

	- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
	    return [currentAlbumData[@"titles"] count];
	}
	
	- (UITableViewCell*)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
	    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"cell"];
	    if (!cell) {
	        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleValue1 reuseIdentifier:@"cell"];
	    }
	
	    cell.textLabel.text = currentAlbumData[@"titles"][indexPath.row];
	    cell.detailTextLabel.text = currentAlbumData[@"values"][indexPath.row];
	
	    return cell;
	}

tableView:numberOfRowsIndexSection: 返回表单视图显示的行数，匹配数据结构中标题的数目。

tableView:cellForRowAtIndexPath: 创建并返回一个带标题和信息的 cell。

现在构建并运行你的项目。你的程序开始运行并显示出下图的界面：

![5](http://cdn1.raywenderlich.com/wp-content/uploads/2013/08/design-pattern-stage1.png)

这目前为止事情看起来很不错。但是如果你回过去看第一张图片的时候，你会发现在屏幕的顶端有一个可以水平滚动的视图，用于切换专辑。它只是简单的水平滚动，为什么不做一个可以重复使用的视图来代替它呢。

![6](这目前为止事情看起来很不错。但是如果你回过去看第一张图片的时候，你会发现在屏幕的顶端有一个可以水平滚动的视图，用于切换专辑。它只是简单的水平滚动，为什么不做一个可以重复使用的视图来代替它呢。)

这个可以重复使用的视图 (view)，它所有的内容都由另一个对象来管理：委托 (delegate)。这个水平滚动区域需要声明一个方法，一个为滚动区域工作的代理工具，就像 UITableView 代理方法如何工作一样。当我讨论下一个设计模式时将会实施它。

**设计模式系列文章**：

[iOS 设计模式系列：开篇](http://www.superqq.com/blog/2015/06/10/ios-she-ji-mo-shi-xi-lie-:kai-pian/)

[iOS 设计模式系列：MVC – 设计模式中的国王](http://www.superqq.com/blog/2015/06/11/ios-she-ji-mo-shi-xi-lie-:mvc-she-ji-mo-shi-zhong-de-guo-wang/)

[iOS 设计模式系列：Singleton – 单例模式](http://www.superqq.com/blog/2015/06/13/ios-she-ji-mo-shi-xi-lie-:singleton-dan-li-mo-shi/)

[iOS 设计模式系列：Facade – 外观模式](http://www.superqq.com/blog/2015/06/15/ios-she-ji-mo-shi-xi-lie-:facade-wai-guan-mo-shi/)


