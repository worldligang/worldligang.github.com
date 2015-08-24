---
layout: post
title: "多个UIImage合并成一个UIImage"
date: 2015-08-05 17:43:53 +0800
comments: true
categories: iOS开发
keywords: UIImage合并, UIImage, Photo Album, iOS开发, 刚刚在线
description: 本篇文章将会介绍如何将多个UIImage合并成一个UIImage。听起来似乎有点晕晕乎乎的，这个可以合并吗？答案是肯定的。我们拿两个UIImage来举例。

---

写了两篇关于照片保存的文章：

1. [iOS开发之保存照片到系统相册（Photo Album）](http://www.superqq.com/blog/2015/08/03/ioskai-fa-zhi-bao-cun-zhao-pian-dao-xi-tong-xiang-ce-%28photo-album%29/) 
2. [iOS开发之保存照片到自己创建的相簿](http://www.superqq.com/blog/2015/08/04/save-photo-to-own-album/)

介绍了如何将图片保存到系统相册，也可以将照片保存到自己的相簿。里面还介绍了一个第三方`ALAssetsLibrary+CustomPhotoAlbum`保存照片的方法。

本篇文章将会介绍如何将多个`UIImage`合并成一个`UIImage`。听起来似乎有点晕晕乎乎的，这个可以合并吗？答案是肯定的。我们拿两个`UIImage`来举例。

<!--more-->

##创建两个UIImage
	
	UIImage *image1 = [UIImage imageNamed:@"iOSDevTip"];
	UIImage *image2 = [UIImage imageNamed:@"CodePush"];

创建`UIImage`的方法有很多种，我们就简单的通过`imageNamed:`方法来创建。

##合并之后的size

	CGSize size = CGSizeMake(image1.size.width + image2.size.width, image1.size.height);

合并两个`UIImage`，需要计算合并之后的`size`。假设这两个`UIImage`的高度是是相同的，把他们的宽度相加，得到合并之后的`UIImage`的`size`。

##合并方法
 
 
有了`UIImage`和`size`接下来就是把两个`UIImage`合并，方法如下：

	 UIGraphicsBeginImageContext(size);
    [image1 drawInRect:CGRectMake(0, 0, image1.size.width, size.height)];
    [image2 drawInRect:CGRectMake(image1.size.width, 0, image2.size.width, size.height)];
    UIImage *togetherImage = UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();

通过上述的代码，就可以实现两个`UIImage`的合并。` 这个小功能还挺有用的。