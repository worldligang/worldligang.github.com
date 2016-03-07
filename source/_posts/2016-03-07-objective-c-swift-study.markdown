---
layout: post
title: Objective-C和Swift混编的一些经验
date: 2016-03-07 22:38:41 +0800
comments: true
categories: 推荐
keywords: Objective, Swift, 程序员  , iOS开发, 刚刚在线
description: 使用Swift作为主要的开发语言，很难避免引入Swift编写的库。2.1.0版本引入了SwiftyJSON和Charts这两个Swift写的库，分别用于处理JSON数据和画监控图。

---

	
> 阿里云iOS客户端2.1.0版本中开始尝试使用Swift来写新的业务，磕磕绊绊总算是发布了新版，总结一下开发过程中得到的经验和踩过的坑吧。

##CocoaPods

使用Swift作为主要的开发语言，很难避免引入Swift编写的库。2.1.0版本引入了SwiftyJSON和Charts这两个Swift写的库，分别用于处理JSON数据和画监控图。

苹果要求使用Swift写的库，必须通过动态链接库引入，其实这一点我也是不太理解的，因为静态库也是可以依赖动态库的符号的，不存在导入多个Swift动态库的问题。允许App使用自带的动态库从iOS8才开始支持，因此必须将App支持的iOS版本升到iOS8。阿里云iOS客户端iOS7的用户不到4%，所以放弃了对iOS7的支持。

Cocoapods支持将依赖的组件编译成动态库，只需要在Podfile顶部加上"use_frameworks!"。开启这个选项之后，所有以源码引入的pod都会编译成动态链接库，而以fake framework引入的pod仍然会编译到主App里面。动态库都放在App里面的Frameworks目录，可以看到Swift相关的动态库也都拷贝进来了，所以支持Swift会导致包变大。我没有记下2.0.0版本的大小，导致没法对比2.1.0放大了多少，这是一个失误。
	
	[~/Library/Developer/Xcode/Archives/2015-12-17/CloudConsoleApp 15-12-17 下午9.24.xcarchive/Products/Applications/CloudConsoleApp.app/Frameworks]$ tree
	.
	├── Charts.framework
	│   ├── Charts
	│   ├── Info.plist
	│   └── _CodeSignature
	│       └── CodeResources
	├── EAIntroView.framework
	│   ├── EAIntroView
	│   ├── Info.plist
	│   └── _CodeSignature
	│       └── CodeResources
	├── FMDB.framework
	│   ├── FMDB
	│   ├── Info.plist
	│   └── _CodeSignature
	│       └── CodeResources
	├── FTCoreText.framework
	│   ├── FTCoreText
	│   ├── Info.plist
	│   └── _CodeSignature
	│       └── CodeResources
	├── JSBadgeView.framework
	│   ├── Info.plist
	│   ├── JSBadgeView
	│   ├── JSBadgeView.bundle
	│   └── _CodeSignature
	│       └── CodeResources
	├── MBProgressHUD.framework
	│   ├── Info.plist
	│   ├── MBProgressHUD
	│   └── _CodeSignature
	│       └── CodeResources
	├── MFSideMenu.framework
	│   ├── Info.plist
	│   ├── MFSideMenu
	│   └── _CodeSignature
	│       └── CodeResources
	├── PFormanceKit.framework
	│   ├── Info.plist
	│   ├── PFormanceKit
	│   └── _CodeSignature
	│       └── CodeResources
	├── Reachability.framework
	│   ├── Info.plist
	│   ├── Reachability
	│   └── _CodeSignature
	│       └── CodeResources
	├── RegexKitLite.framework
	│   ├── Info.plist
	│   ├── RegexKitLite
	│   └── _CodeSignature
	│       └── CodeResources
	├── SSZipArchive.framework
	│   ├── Info.plist
	│   ├── SSZipArchive
	│   └── _CodeSignature
	│       └── CodeResources
	├── SnapKit.framework
	│   ├── Info.plist
	│   ├── SnapKit
	│   └── _CodeSignature
	│       └── CodeResources
	├── SwiftyJSON.framework
	│   ├── Info.plist
	│   ├── SwiftyJSON
	│   └── _CodeSignature
	│       └── CodeResources
	├── libswiftContacts.dylib
	├── libswiftCore.dylib
	├── libswiftCoreData.dylib
	├── libswiftCoreGraphics.dylib
	├── libswiftCoreImage.dylib
	├── libswiftDarwin.dylib
	├── libswiftDispatch.dylib
	├── libswiftFoundation.dylib
	├── libswiftObjectiveC.dylib
	└── libswiftUIKit.dylib
	
	27 directories, 49 files
	
	$ file Charts 
	Charts: Mach-O universal binary with 2 architectures
	Charts (for architecture armv7):    Mach-O dynamically linked shared library arm
	Charts (for architecture arm64):    Mach-O 64-bit dynamically linked shared library

因为fake framework和源代码pod分别会编译成静态库和动态库，这样会导致一个问题，就是如果源码pod又依赖fake framework，那就没办法了。CocoaPods发现这种情况会提示下面这个错误。

	target has transitive dependencies that include static binaries: (xxx.framework, xxx.framework)

pod install时会把静态库编译到App里面，源码编译成的动态库没法依赖它。最终的解决方案只能是CocoaPods对fake framework和源码pod一视同仁，都编译成动态库，这样彼此才能依赖。不知道CocoaPods什么时候会支持这样。

##混编

* Swift使用Objective-C

这种情况占绝大多数。只需要在CloudConsoleApp-Bridging-Header.h这个头文件中包含相关的头文件就行。pod组件另外一种引入的方式是通过@import引入。比如SDWebImage可以通过下面两种方式引入。

	//在Bridging头文件包含下面这个头文件
	#import <SDWebImage/UIImageView+WebCache.h>
	
	//另外一种办法，在Swift文件中引入。
	import SDWebImage

Objective-C写的类和方法都会被改成Swift的使用方式，下面是两个很典型的例子。使用的时候需要尝试一下才能找到翻译的Swift方法。

	//Objective-C
	titleLabel.lineBreakMode = NSLineBreakByWordWrapping;
	titleLabel.numberOfLines = 0;
	
	//Swift
	cell.nameLabel?.lineBreakMode = .ByWordWrapping //全写是 NSLineBreakMode.ByWordWrapping
	cell.nameLabel?.numberOfLines = 0
	
	//Objective-C
	UIImage *image = [UIImage imageNamed:@"abc"];
	
	//Swift
	let image = UIImage(named: "abc")

* Objective-C使用Swift

Xcode会生成一个虚拟的头文件CloudConsoleApp-Swift.h，在工程里面是找不到这个头文件的，但是可以包含，并且跳转进去。这个文件里面包含了所有从Swift导出来的符号，比如下面这个view controller就是用Swift写的。可以看出来Objectivew-C看到的名称跟Swift源码里面的名称是一样的，但是Swift会对类做demangling，变成了_TtC15CloudConsoleApp31YWSResourceDetailViewController这样的，跟C++有点类似。
	
	SWIFT_CLASS("_TtC15CloudConsoleApp31YWSResourceDetailViewController")
	@interface YWSResourceDetailViewController : UIPageViewController
	@property (nonatomic, strong) NSArray * __nonnull vcs;
	@property (nonatomic, copy) NSString * __null_unspecified pluginId;
	@property (nonatomic, strong) YWSSegmentedControl * __nonnull segmentedControl;
	@property (nonatomic, strong) YWSInstanceListViewController * __nonnull instanceListViewController;
	@property (nonatomic, strong) YWSMetricConcernedViewController * __nonnull metricConcernedViewController;
	@property (nonatomic, weak) IBOutlet UIBarButtonItem * __null_unspecified addMetricBarButton;
	- (void)viewDidLoad;
	- (void)initSegmentedControl;
	- (void)indexChanged:(id __nonnull)sender;
	- (void)onNavigationBack:(id __nonnull)sender;
	- (void)prepareForSegue:(UIStoryboardSegue * __nonnull)segue sender:(id __nullable)sender;
	- (nonnull instancetype)initWithTransitionStyle:(UIPageViewControllerTransitionStyle)style navigationOrientation:(UIPageViewControllerNavigationOrientation)navigationOrientation options:(NSDictionary<NSString *, id> * __nullable)options OBJC_DESIGNATED_INITIALIZER;
	- (nullable instancetype)initWithCoder:(NSCoder * __nonnull)coder OBJC_DESIGNATED_INITIALIZER;
	@end

##Swift的优缺点

这个项目刚起步，用Swift的经验尚浅，所以都是一些比较浅薄的理解，后面有更深刻的理解再补上。

###优点

* 代码简洁。类的声明和实现在一个文件中。
* 统一对属性和方法的调用，都用.。
* 如果不加额外的访问控制，所有的符号都是整个项目可见，无需考虑头文件的问题。
* 字符串处理太方便了。
	
	//字符串比较和拼接实在是太方便了
	let foo = "abc"
	let bar = "abc"
	
	if foo == bar {
	    //blablabla
	}
	
	print("====\(foo)+\(bar)")

* 语言上支持延迟加载。

	lazy var imageView : UIImageView = {
	    var imageView = UIImageView(image: UIImage(named: "empty_hint"))
	    imageView.contentMode = .ScaleAspectFit
	
	    return imageView
	}()
	
	lazy var infoLabel : UILabel = {
	    var infoLabel = UILabel()
	    infoLabel.lineBreakMode = .ByWordWrapping //支持换行
	    infoLabel.numberOfLines = 0
	
	    return infoLabel
	}()
	
	lazy var button : UIButton = {
	    var button = UIButton()
	    button.titleLabel?.font = UIFont.systemFontOfSize(15)
	    button.setTitleColor(UIColor.darkGrayColor(), forState: .Normal)
	    button.setBackgroundImage(UIImage(named: "buy_instance_hint_button"), forState: .Normal)
	    button.hidden = true
	
	    return button
	}()

* 多返回值。比如下面这个函数，如果使用Objective-C写还是比较麻烦的。

	//将 "创建中&#FA8C35" 翻译成对应的 "(字符串对象, 颜色对象)"
	func YWSTranslateRichText (str : String) -> (text : String, color : UIColor) {
	    let statusArray = str.componentsSeparatedByString("&")
	
	    if statusArray.count == 0 {
	        return ("", UIColor.lightGrayColor())
	    }
	
	    if statusArray.count == 1 {
	        return (statusArray[0], UIColor.lightGrayColor())
	    }
	
	    return (statusArray[0], UIColor.fromHexString(statusArray[1]))
	}
	
	//使用方式如下
	let (text, color) = YWSTranslateRichText(instanceStatusConf)

* 支持字符串作为枚举值。

	enum YWSECSInstanceStatus : String {
	    case Starting = "Starting"
	    case Running = "Running"
	    case Stopping = "Stopping"
	    case Stopped = "Stopped"
	}
	
	//使用方法
	cell.ECSInstanceStatus = YWSECSInstanceStatus(rawValue: instanceStatus!)
	
	//转换成字符串
	textDetailLabel.text = YWSECSInstanceStatus.Starting.rawValue


* selector类型实现了 StringLiteralConvertible，使用起来更加简单。

	self.button.addTarget(self, action: "introduceResources:", forControlEvents: .TouchUpInside)

* 不再需要引入libextobjc这个Pod，因为Swift支持更方便的用法。在block开始的时候，在数组里面weak所有要用到的对象。

	inputViewController.finishBlock = { [weak inputViewController, weak cell, weak self] () -> Void in
	}

* 函数支持默认参数。比如下面这个函数，有五个参数，其中三个有默认参数，用户需要设置的参数只有两个。

	convenience init(text: String, textColor: UIColor = UIColor.whiteColor(), bgColor: UIColor, font: UIFont = UIFont.systemFontOfSize(10), inset: UIEdgeInsets = UIEdgeInsetsMake(0, 3, 0, 3)) {
	    //blabla
	}
	
	lazy var vipLabel : YWSInsetsTextLabel! = YWSInsetsTextLabel(text: "vip", bgColor: UIColor.orangeColor())

* 通过减少动态性，使用vtable替换原有的objc_msgSend，获取更高的性能。新增了final、private等关键字，编译器可以对代码做更多优化，提升性能并减少内存的使用。比如final方法不用放入虚表中，节省内存；跳转时不用查表，性能更佳；private的类如果发现有方法只在本文件中使用，可以直接内联，提高性能。

* 在Playground工程里面练习Swift编程非常之方便，尤其是测试VFL语句的时候。


![1](http://upload-images.jianshu.io/upload_images/1376176-76d2627719e67fa6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##缺点

* Optional让人头疼，大量的?和!，没处理好很容易导致崩溃。
* 强类型和Optional，给JSON解释带来了灾难。
* 目前Xcode不支持对Swift写的代码做重构。

* Build Settings里面设置Treat Warnings as Errors对Swift代码无效。

##坑

用private修饰的类，如果使用KVC来给属性设置值，编译不会报错，运行时也不会报错，但就是设置不上。去掉private就好了。

##crash分析
手解crash可以看到具体崩溃代码的行号。

	$ symbolicatecrash ~/Downloads/034dc058c5d4ff1f717ec7a05d4d55b8 CloudConsoleApp.app.dSYM
	
	Exception Type:  SIGTRAP
	Exception Codes: #0 at 0x1001c09b4
	Crashed Thread:  0
	
	Thread 0 Crashed:
	0   CloudConsoleApp                     0x00000001001c09b4 YWSInstanceListViewController.goToBuyPage() -> () (YWSInstanceListViewController.swift:701)
	1   CloudConsoleApp                     0x00000001001c92cc specialized YWSInstanceListViewController.introduceResources(AnyObject) -> () (YWSInstanceListViewController.swift:689)
	2   CloudConsoleApp                     0x00000001001c029c @objc YWSInstanceListViewController.introduceResources(AnyObject) -> () (YWSInstanceListViewController.swift:0)
	3   UIKit                               0x000000018601be50 0x185fd0000 + 310864
	4   UIKit                               0x000000018601bdcc 0x185fd0000 + 310732
	5   UIKit                               0x0000000186003a88 0x185fd0000 + 211592
	6   UIKit                               0x000000018601b6e4 0x185fd0000 + 308964
	7   UIKit                               0x000000018601b314 0x185fd0000 + 307988
	8   UIKit                               0x0000000186013e30 0x185fd0000 + 278064
	9   UIKit                               0x0000000185fe44cc 0x185fd0000 + 83148
	10  UIKit                               0x0000000185fe2794 0x185fd0000 + 75668
	11  CoreFoundation                      0x00000001812a8efc 0x1811cc000 + 904956
	12  CoreFoundation                      0x00000001812a8990 0x1811cc000 + 903568
	13  CoreFoundation                      0x00000001812a6690 0x1811cc000 + 894608
	14  CoreFoundation                      0x00000001811d5680 0x1811cc000 + 38528
	15  GraphicsServices                    0x00000001826e4088 0x1826d8000 + 49288
	16  UIKit                               0x000000018604cd90 0x185fd0000 + 511376
	17  CloudConsoleApp                     0x000000010014b4e0 main (main.m:16)
	18  libdyld.dylib                       0x0000000180d768b8 0x180d74000 + 10424
	
	//不过我对着这行代码分析了好久，实在想不出来崩溃的原因。没有任何crash提示信息。
	//这个版本Swift代码只有这样一个crash
	//后面再看看新crash会不会也是这样
	self.resourceType = YWSXXX.shareInstance().getXXXByXXX(self.XXX.pluginId)

实际证明Swift的crash信息非常不准确，能知道崩溃的文件和函数，行号不准确，也不会输出Application Specific Information。比如下面这个crash。

	Incident Identifier: 54087A46-D37D-454B-9305-22ED5420B58B
	CrashReporter Key:   TODO
	Hardware Model:      iPhone6,2
	Process:             CloudConsoleApp [696]
	Path:                /var/mobile/Containers/Bundle/Application/E8E24C8B-A47B-425E-863F-A871F273FCA2/CloudConsoleApp.app/CloudConsoleApp
	Identifier:          com.aliyun.wstudio.amc.AliyunMobileApp
	Version:             2.2.0 (2169)
	Code Type:           ARM-64
	Parent Process:      ??? [1]
	
	Date/Time:           2016-01-25 10:44:56 +0000
	OS Version:          iPhone OS 9.2.1 (13D15)
	Report Version:      104
	
	Exception Type:  SIGTRAP
	Exception Codes: #0 at 0x100139e80
	Triggered by Thread:  0
	
	Thread 0 Crashed:
	0   CloudConsoleApp                 0x0000000100139e80 __TFC15CloudConsoleApp24YWSTouchIDViewController14viewWillAppearfS0_FSbT_ (in CloudConsoleApp) + 1304
	1   CloudConsoleApp                 0x0000000100139eb0 __TToFC15CloudConsoleApp24YWSTouchIDViewController14viewWillAppearfS0_FSbT_ (in CloudConsoleApp) + 44
	2   UIKit                           0x000000018722c74c 0x0000000187200000 + 182092
	3   UIKit                           0x000000018722c4c0 0x0000000187200000 + 181440
	4   UIKit                           0x00000001872d3130 0x0000000187200000 + 864560
	5   UIKit                           0x00000001872d2a6c 0x0000000187200000 + 862828
	6   UIKit                           0x00000001872d2694 0x0000000187200000 + 861844
	7   UIKit                           0x00000001872d25fc 0x0000000187200000 + 861692
	8   UIKit                           0x000000018720f778 0x0000000187200000 + 63352
	9   QuartzCore                      0x0000000184c1eb2c 0x0000000184c10000 + 60204
	10  QuartzCore                      0x0000000184c19738 0x0000000184c10000 + 38712
	11  QuartzCore                      0x0000000184c195f8 0x0000000184c10000 + 38392
	12  QuartzCore                      0x0000000184c18c94 0x0000000184c10000 + 35988
	13  QuartzCore                      0x0000000184c189dc 0x0000000184c10000 + 35292
	14  QuartzCore                      0x0000000184c120cc 0x0000000184c10000 + 8396
	15  CoreFoundation                  0x00000001824d8588 0x00000001823fc000 + 902536
	16  CoreFoundation                  0x00000001824d632c 0x00000001823fc000 + 893740
	17  CoreFoundation                  0x00000001824d675c 0x00000001823fc000 + 894812
	18  CoreFoundation                  0x0000000182405680 0x00000001823fc000 + 38528
	19  GraphicsServices                0x0000000183914088 0x0000000183908000 + 49288
	20  UIKit                           0x000000018727cd90 0x0000000187200000 + 511376
	21  CloudConsoleApp                 0x000000010007f988 main (in CloudConsoleApp) (main.m:16)
	22  libdyld.dylib                   0x0000000181fa68b8 0x0000000181fa4000 + 10424
	
	//确实崩溃在viewWillAppear函数中，但是是97行的 as! 导致的，在crash信息里面这两个重要的信息没有暴露出来。
	override func viewWillAppear(animated: Bool) {
	    super.viewWillAppear(animated)
	
	    //let urlString = YWSXXX.sharedInstance().get("userIcon") as! String
	
	    //因为2.1.0重构过登录模块，新的登录模块才会保存 userIcon 这个值
	    //所以2.2.0覆盖2.1.0之前的版本用 as! 会崩溃，这里改成 as? 了
	    if let urlString = YWSXXX.sharedInstance().get("userIcon") as? String {
	    }
	}

##总结

Swift 2.1版本已经非常稳定，苹果将其开源，也表明对Swift的质量和可靠性有足够的信心。开源社区开始涌现一批优秀的Swift库，比如Charts，这个画图的组件很不错。StackOverflow的答案中很多人会同时提供Objective-C和Swift两个版本。目前来看唯一美中不足的问题就是解出来的crash没有Objective-C那么直观了，很多时候都得靠猜。

	文／阿呆少爷（简书作者）
	原文链接：http://www.jianshu.com/p/a5e6e574145b
