---
layout: post
title: "iOS开发之UIImage等比缩放"
date: 2015-07-29 00:01:15 +0800
comments: true
categories: iOS开发
keywords: UIImage, geometric,缩放, iOS开发, 刚刚在线
description: 前面讲了截取UIImage指定大小区域，很方便的截取`UIImage`。今天要和大家分享的是`UIImage`的缩放。

---

##评论功能真不错

评论开通后，果然有很多人吐槽。谢谢大家的支持和关爱，如果有做的不到的地方，还请海涵。毕竟我一个人的力量是有限的，我会尽自己最大的努力大家准备一些干货。

有些内容可能都是比较基础的，记住：不积跬步无以至千里，不积小流无以成江海。我想这个道理大家都懂，在为大家准备文章的过程中，对我来说也是一次学习成长。

大家可以继续吐槽！微信的评论就是一个沟通的渠道，也是一篇文章的延续。你也可以在评论中写一些你对iOS开发的见解，说不定就会对他人有帮助。

很高兴，已经有好几位iOS开发的朋友答应以后会写文章投稿给我。在这里，我再强调一下：如果你有好的技术文章一定要投稿给我噢！投稿邮箱：

	worldligang@163.com

<!--more-->

前面讲了[截取UIImage指定大小区域](http://www.superqq.com/blog/2015/07/26/jie-qu-uiimagezhi-ding-da-xiao-qu-yu/)，很方便的截取`UIImage`。今天要和大家分享的是`UIImage`的缩放。

两种缩放:

1. 缩放到指定大小，也就是指定的`size`.
2. 等比缩放。
	
##缩放到指定大小
	
	- (UIImage*)imageCompressWithSimple:(UIImage*)image scaledToSize:(CGSize)size
	{
	    UIGraphicsBeginImageContext(size);
	    [image drawInRect:CGRectMake(0,0,size.width,size.height)];
	    UIImage* newImage = UIGraphicsGetImageFromCurrentImageContext();
	    UIGraphicsEndImageContext();
	    return newImage;
	}
	
##等比缩放
	
###通过缩放系数
	
	- (UIImage*)imageCompressWithSimple:(UIImage*)image scale:(float)scale
	{
	    CGSize size = image.size;
	    CGFloat width = size.width;
	    CGFloat height = size.height;
	    CGFloat scaledWidth = width * scale;
	    CGFloat scaledHeight = height * scale;
	    UIGraphicsBeginImageContext(size); // this will crop
	    [image drawInRect:CGRectMake(0,0,scaledWidth,scaledHeight)];
	    UIImage* newImage= UIGraphicsGetImageFromCurrentImageContext();
	    UIGraphicsEndImageContext();
	    return newImage;
	}
	
`scale`是缩放系数	。

###通过计算得到缩放系数
		
	- (UIImage*)imageByScalingAndCroppingForSize:(CGSize)targetSize
	{
	    
	    UIImage *sourceImage = [UIImage imageNamed:@"test.jpg"];
	    UIImage *newImage = nil;
	    CGSize imageSize = sourceImage.size;
	    CGFloat width = imageSize.width;
	    CGFloat height = imageSize.height;
	    CGFloat targetWidth = targetSize.width;
	    CGFloat targetHeight = targetSize.height;
	    CGFloat scaleFactor = 0.0;
	    CGFloat scaledWidth = targetWidth;
	    CGFloat scaledHeight = targetHeight;
	    CGPoint thumbnailPoint = CGPointMake(0.0,0.0);
	    
	    if (CGSizeEqualToSize(imageSize, targetSize) == NO)
	    {
	        CGFloat widthFactor = targetWidth / width;
	        CGFloat heightFactor = targetHeight / height;
	        if (widthFactor > heightFactor)
	            scaleFactor = widthFactor; // scale to fit height
	        else
	            scaleFactor = heightFactor; // scale to fit width
	        
	        scaledWidth= width * scaleFactor;
	        scaledHeight = height * scaleFactor;
	        // center the image
	        if (widthFactor > heightFactor)
	        {
	            thumbnailPoint.y = (targetHeight - scaledHeight) * 0.5;
	        }
	        else if (widthFactor < heightFactor)
	        {
	            thumbnailPoint.x = (targetWidth - scaledWidth) * 0.5;
	        }
	    }
	    
	    UIGraphicsBeginImageContext(targetSize); // this will crop
	    CGRect thumbnailRect = CGRectZero;
	    thumbnailRect.origin = thumbnailPoint;
	    thumbnailRect.size.width= scaledWidth;
	    thumbnailRect.size.height = scaledHeight;
	    [sourceImage drawInRect:thumbnailRect];
	    newImage = UIGraphicsGetImageFromCurrentImageContext();
	    
	    if(newImage == nil)
	        NSLog(@"could not scale image");
	    //pop the context to get back to the default
	    UIGraphicsEndImageContext();
	    
	    return newImage;
	    
	}

很久之前写了一篇文章[解决MWPhotoBrowser中的SDWebImage加载大图导致的内存警告问题](http://www.superqq.com/blog/2015/01/22/jie-jue-mwphotobrowserzhong-de-sdwebimagejia-zai-da-tu-dao-zhi-de-nei-cun-jing-gao-wen-ti/)。这个我记得当时从服务器拿到的照片大概有10几M的样子，加载出来会导致内存警告。所以我当时通过修改SDWebImage源码，就是把下载下来的照片进行缩放，内存就降下来了。一般情况下应该不会加载这么大的照片的，用户要是知道，早把你的APP给删掉了。
	
