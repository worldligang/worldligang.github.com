---
layout: post
title: "截取UIImage指定大小区域"
date: 2015-07-26 10:07:58 +0800
comments: true
categories: iOS开发
keywords: 截取UIImage, iOS UIImage, UIImage, iOS开发, 刚刚在线
description: 从服务器获取到一张照片，只需要显示他的左半部分，或者中间部分等等。也就是截取UIImage指定大小区域。

---

最近遇到这样的需求：从服务器获取到一张照片，只需要显示他的左半部分，或者中间部分等等。也就是截取UIImage指定大小区域。

##UIImage扩展

我的解决方案是对`UIImage`进行扩展。通过`CGImageRef`和`CGImage`完成截取，调用的方法是：`CGImageCreateWithImageInRect`。扩展类叫`UIImage+Crop`，具体代码如下：

###UIImage+Crop.h

	#import <UIKit/UIKit.h>
	
	typedef NS_ENUM(NSInteger, XYCropImageStyle){
	    XYCropImageStyleRight               =0,      // 右半部分
	    XYCropImageStyleCenter              =1,      // 中间部分
	    XYCropImageStyleLeft                =2,      // 左半部分
	    XYCropImageStyleRightOneOfThird     =3,      // 右侧三分之一部分
	    XYCropImageStyleCenterOneOfThird    =4,      // 中间三分之一部分
	    XYCropImageStyleLeftOneOfThird      =5,      // 左侧三分之一部分
	    XYCropImageStyleRightQuarter        =6,      // 右侧四分之一部分
	    XYCropImageStyleCenterRightQuarter  =7,      // 中间右侧四分之一部分
	    XYCropImageStyleCenterLeftQuarter   =8,      // 中间左侧四分之一部分
	    XYCropImageStyleLeftQuarter         =9,      // 左侧四分之一部分
	};
	
	@interface UIImage (Crop)
	- (UIImage *)imageByCroppingWithStyle:(XYCropImageStyle)style;
	
	@end

###UIImage+Crop.m

	#import "UIImage+Crop.h"
	
	@implementation UIImage (Crop)
	
	- (UIImage *)imageByCroppingWithStyle:(XYCropImageStyle)style
	{
	    CGRect rect;
	    switch (style) {
	        case XYCropImageStyleLeft:
	            rect = CGRectMake(0, 0, self.size.width/2, self.size.height);
	            break;
	        case XYCropImageStyleCenter:
	            rect = CGRectMake(self.size.width/4, 0, self.size.width/2, self.size.height);
	            break;
	        case XYCropImageStyleRight:
	            rect = CGRectMake(self.size.width/2, 0, self.size.width/2, self.size.height);
	            break;
	        case XYCropImageStyleLeftOneOfThird:
	            rect = CGRectMake(0, 0, self.size.width/3, self.size.height);
	            break;
	        case XYCropImageStyleCenterOneOfThird:
	            rect = CGRectMake(self.size.width/3, 0, self.size.width/3, self.size.height);
	            break;
	        case XYCropImageStyleRightOneOfThird:
	            rect = CGRectMake(self.size.width/3*2, 0, self.size.width/3, self.size.height);
	            break;
	        case XYCropImageStyleLeftQuarter:
	            rect = CGRectMake(0, 0, self.size.width/4, self.size.height);
	            break;
	        case XYCropImageStyleCenterLeftQuarter:
	            rect = CGRectMake(self.size.width/4, 0, self.size.width/4, self.size.height);
	            break;
	        case XYCropImageStyleCenterRightQuarter:
	            rect = CGRectMake(self.size.width/4*2, 0, self.size.width/4, self.size.height);
	            break;
	        case XYCropImageStyleRightQuarter:
	            rect = CGRectMake(self.size.width/4*3, 0, self.size.width/4, self.size.height);
	            break;
	        default:
	            break;
	    }
	    CGImageRef imageRef = self.CGImage;
	    CGImageRef imagePartRef = CGImageCreateWithImageInRect(imageRef, rect);
	    UIImage *cropImage = [UIImage imageWithCGImage:imagePartRef];
	    CGImageRelease(imagePartRef);
	    return cropImage;
	}

##实际运用

简单测试一下，看看有没有实现我们想要的效果。首先，先加载一个完整的UIImageView。这个应该不难。代码如下：

	UIImageView *imgView = [[UIImageView alloc] init];
    imgView.frame = CGRectMake((SCREEN.width - 226) / 2, 100, 226, 106);
    UIImage *image = [UIImage imageNamed:@"ganggang"];
    imgView.image = image;
    [self.view addSubview:imgView];
    
运行一下：

![jiequing](http://7xkkk9.com1.z0.glb.clouddn.com/jiequing.png)

要对UIImage进行裁剪，首先导入头文件：

	#import "UIImage+Crop.h"

在上面`UIImage *image = [UIImage imageNamed:@"ganggang"];`这段代码之后加上下面这句：

    image = [image imageByCroppingWithStyle:XYCropImageStyleLeft];
    
`XYCropImageStyleLeft`是截取照片的左半部分。效果如下：

![jiequed](http://7xkkk9.com1.z0.glb.clouddn.com/jiequed.png)

截取成功，还可以截取其他区域的，只需要传入不同的`XYCropImageStyle`即可实现。以上代码依然在[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)里。

