---
layout: post
title: "自定义导航按钮UIBarButtonItem"
date: 2015-08-02 00:13:14 +0800
comments: true
categories: iOS开发
keywords: 导航, UIBarButtonItem, 自定义导航, iOS开发, 刚刚在线
description: 基本上每个iOSAPP里面都有导航，比如微信、QQ、支付宝。导航可以很方便地帮助我们管理视图控制器（UIViewController）。导航的重要性不言而喻，基本上是每一位iOS初学者都要接触到的问题。

---

基本上每个iOS APP里面都有导航，比如微信、QQ、支付宝。导航可以很方便地帮助我们管理视图控制器（UIViewController）。导航的重要性不言而喻，基本上是每一位iOS初学者都要接触到的问题。

iOS系统导航栏中有`leftBarButtonItem`和`rightBarButtonItem`，我们可以根据自己的需求来自定义这两个`UIBarButtonItem`。

<!--more-->

##四种创建方法

系统提供了四种创建的方法：

	- (instancetype)initWithBarButtonSystemItem:(UIBarButtonSystemItem)systemItem target:(id)target action:(SEL)action;


	- (instancetype)initWithImage:(UIImage *)image style:(UIBarButtonItemStyle)style target:(id)target action:(SEL)action;
	

	- (instancetype)initWithTitle:(NSString *)title style:(UIBarButtonItemStyle)style target:(id)target action:(SEL)action;

	- (instancetype)initWithBarButtonSystemItem:(UIBarButtonSystemItem)systemItem target:(id)target action:(SEL)action;

	- (instancetype)initWithCustomView:(UIView *)customView;

###通过系统UIBarButtonSystemItem创建

自定义`rightBarButtonItem`，代码如下：

    self.navigationItem.rightBarButtonItem = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemDone target:self action:@selector(right:)];

UIBarButtonSystemItem有以下样式可以供选择：
	
	typedef NS_ENUM(NSInteger, UIBarButtonSystemItem) {
	    UIBarButtonSystemItemDone,
	    UIBarButtonSystemItemCancel,
	    UIBarButtonSystemItemEdit,  
	    UIBarButtonSystemItemSave,  
	    UIBarButtonSystemItemAdd,
	    UIBarButtonSystemItemFlexibleSpace,
	    UIBarButtonSystemItemFixedSpace,
	    UIBarButtonSystemItemCompose,
	    UIBarButtonSystemItemReply,
	    UIBarButtonSystemItemAction,
	    UIBarButtonSystemItemOrganize,
	    UIBarButtonSystemItemBookmarks,
	    UIBarButtonSystemItemSearch,
	    UIBarButtonSystemItemRefresh,
	    UIBarButtonSystemItemStop,
	    UIBarButtonSystemItemCamera,
	    UIBarButtonSystemItemTrash,
	    UIBarButtonSystemItemPlay,
	    UIBarButtonSystemItemPause,
	    UIBarButtonSystemItemRewind,
	    UIBarButtonSystemItemFastForward,
	#if __IPHONE_3_0 <= __IPHONE_OS_VERSION_MAX_ALLOWED
	    UIBarButtonSystemItemUndo,
	    UIBarButtonSystemItemRedo,
	#endif
	#if __IPHONE_4_0 <= __IPHONE_OS_VERSION_MAX_ALLOWED
	    UIBarButtonSystemItemPageCurl,
	#endif
	};
	

最后别忘了实现`right:`方法：

	- (void)right:(id)sender
	{
	    NSLog(@"rightBarButtonItem");
	}


###自定义文字的UIBarButtonItem

在文章[关于导航栏的六个小技巧](http://www.superqq.com/blog/2015/07/30/six-tips-on-navigation-bar/)的第五个技巧里面有自定义`rightBarButtonItem`

	self.navigationItem.leftBarButtonItem = [[UIBarButtonItem alloc] initWithTitle:@"back" style:UIBarButtonItemStylePlain target:self action:@selector(back:)];

UIBarButtonItemStyle有以下三种选择：

	typedef NS_ENUM(NSInteger, UIBarButtonItemStyle) {
	    UIBarButtonItemStylePlain,
	    UIBarButtonItemStyleBordered NS_ENUM_DEPRECATED_IOS(2_0, 8_0, "Use UIBarButtonItemStylePlain when minimum deployment target is iOS7 or later"),
	    UIBarButtonItemStyleDone,
	};

实现`back:`方法：
	
	- (void)back:(id)sender
	{
	    [self.navigationController popViewControllerAnimated:YES];
	}

###自定义照片的UIBarButtonItem

    self.navigationItem.rightBarButtonItem = [[UIBarButtonItem alloc] initWithImage:[UIImage imageNamed:@"test"] style:UIBarButtonItemStylePlain target:self action:@selector(right:)];


###自定义UIView的UIBarButtonItem

自定义`UIView`，然后通过`initWithCustomView:`方法来创建`UIBarButtonItem`。

	
 	UIView *testView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 40, 60)];
    self.navigationItem.rightBarButtonItem = [[UIBarButtonItem alloc] initWithCustomView:testView];
    
看到有朋友在后台提问：

	刚哥，我现在即需要改那个导航原生的返回图片，也要改返回文字，应该怎么改呢，求指教。

其实，这个就可以用`initWithCustomView:`来解决，自定义`UIView`你可以放`UIImageView`和`UILabel`。可以自定义`UIView`，那么想怎么定义都是可以的。
 
