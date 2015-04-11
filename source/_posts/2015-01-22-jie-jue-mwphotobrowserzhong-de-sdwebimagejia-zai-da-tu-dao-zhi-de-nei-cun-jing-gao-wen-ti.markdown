---
layout: post
title: "解决MWPhotoBrowser中的SDWebImage加载大图导致的内存警告问题"
date: 2015-01-22 11:23:21 +0800
comments: true
categories: iOS开发
keywords: MWPhotoBrowser, SDWebImage, Cocoapods, 内存警告, SDWebImage内存警告, MWPhotoBrowser内存警告, 客户端, iOS, iOS开发, cocoapods, 个人博客, 刚刚在线
description: "解决MWPhotoBrowser中的SDWebImage加载大图导致的内存警告问题"

---

MWPhotoBrowser是一个非常不错的照片浏览器，在github的star接近3000个，<a href="https://github.com/mwaterfall/MWPhotoBrowser.git"target="_blank"title="进入官网">MWPhotoBrowser下载</a> 

MWPhotoBrowser来加载小图1M以下的都应该不会有内存警告的问题。如果遇到大图，3M、4M、5M的大图，很有可能导致内存警告。最近我就遇到这个问题，很是头疼。来回滑动查看照片内存飙到100M以上：

<img src="http://mmbiz.qpic.cn/mmbiz/8RTSPr4mlykHge8gs1dCibkoTYXiatCCTMGicKA3aLRCFW6HzgMFvlzGiagwGrEN6hSw6Lt8y13WuFZORDic4hWbRsg/640" /></a>

网上查了很多资料，都没有解决问题。

我们来看一下MWPhotoBrowser，其实MWPhotoBrowser用的是SDWebImage来下载图片的。<a href=" https://github.com/rs/SDWebImage.git"target="_blank"title="进入官网">SDWebImage下载</a>

在github看到SDWebImage的介绍，后面说到：

    Future Enhancements

        LRU memory cache cleanup instead of reset on memory warning

看到这个真是欲哭无泪啊。

再去看看SDWebImage的，有个人提问了：

    How to disable "memory cache"?  I don't want memory cache,  it used a lot of memory and got memory waring easily,  disk is enough for me...

有人回答：

    There is no way to disable the memory cache. But the cache is designed to flush itself when you get a memory warning, so you shouldn't need to worry it.

说的是SDWebImage遇到内存警告会自动释放内存，但是这还是解决不了问题，加载大图的时候，内存会突然蹦到100多M，在4s及以下的手机上跑，再就挂了。

还是没有解决内存警告的问题。怎么办呢？

我是这么解决的：

SDWebImage有一个SDWebImageDownloaderOperation类来执行下载操作的。里面有个下载完成的方法：

    - (void)connectionDidFinishLoading:(NSURLConnection *)aConnection {
       SDWebImageDownloaderCompletedBlock completionBlock = self.completedBlock;
       @synchronized(self) {
           CFRunLoopStop(CFRunLoopGetCurrent());
           self.thread = nil;
           self.connection = nil;
           [[NSNotificationCenter defaultCenter] postNotificationName:SDWebImageDownloadStopNotification object:nil];
       }
       
       if (![[NSURLCache sharedURLCache] cachedResponseForRequest:_request]) {
           responseFromCached = NO;
       }
       
       if (completionBlock)
       {
           if (self.options & SDWebImageDownloaderIgnoreCachedResponse && responseFromCached) {
               completionBlock(nil, nil, nil, YES);
           }
           else {
               UIImage *image = [UIImage sd_imageWithData:self.imageData];
               NSString *key = [[SDWebImageManager sharedManager] cacheKeyForURL:self.request.URL];
               image = [self scaledImageForKey:key image:image];
               
               // Do not force decoding animated GIFs
               if (!image.images) {
                   image = [UIImage decodedImageWithImage:image];
               }
               if (CGSizeEqualToSize(image.size, CGSizeZero)) {
                   completionBlock(nil, nil, [NSError errorWithDomain:@"SDWebImageErrorDomain" code:0 userInfo:@{NSLocalizedDescriptionKey : @"Downloaded image has 0 pixels"}], YES);
               }
               else {
                   completionBlock(image, self.imageData, nil, YES);
               }
           }
       }
       self.completionBlock = nil;
       [self done];
    }

其中，UIImage *image = [UIImage sd_imageWithData:self.imageData];就是将data转换成image。

再看看sd_imageWithData:这个方法：

    + (UIImage *)sd_imageWithData:(NSData *)data {
       UIImage *image;
       NSString *imageContentType = [NSData sd_contentTypeForImageData:data];
       if ([imageContentType isEqualToString:@"image/gif"]) {
           image = [UIImage sd_animatedGIFWithData:data];
       }
    #ifdef SD_WEBP
       else if ([imageContentType isEqualToString:@"image/webp"])
       {
           image = [UIImage sd_imageWithWebPData:data];
       }
    #endif
       else {
           image = [[UIImage alloc] initWithData:data];
           UIImageOrientation orientation = [self sd_imageOrientationFromImageData:data];
           if (orientation != UIImageOrientationUp) {
               image = [UIImage imageWithCGImage:image.CGImage
                                           scale:image.scale
                                     orientation:orientation];
           }
       }


       return image;
    }

这个方法在UIImage+MultiFormat里面，是UIImage的一个类别处理。这句话很重要image = [[UIImage alloc] initWithData:data]; SDWebImage把下载下来的data直接转成image，然后没做等比缩放直接存起来使用。所以，我们只需要在这边做处理即可：

UIImage+MultiFormat添加一个方法：

    +(UIImage *)compressImageWith:(UIImage *)image
    {
       float imageWidth = image.size.width;
       float imageHeight = image.size.height;
       float width = 640;
       float height = image.size.height/(image.size.width/width);

       float widthScale = imageWidth /width;
       float heightScale = imageHeight /height;
       
       // 创建一个bitmap的context
       // 并把它设置成为当前正在使用的context
       UIGraphicsBeginImageContext(CGSizeMake(width, height));
       
       if (widthScale > heightScale) {
           [image drawInRect:CGRectMake(0, 0, imageWidth /heightScale , height)];
       }
       else {
           [image drawInRect:CGRectMake(0, 0, width , imageHeight /widthScale)];
       }
       
       // 从当前context中创建一个改变大小后的图片
       UIImage *newImage = UIGraphicsGetImageFromCurrentImageContext();
       // 使当前的context出堆栈
       UIGraphicsEndImageContext();
       
       return newImage;
       
    }

然后在：image = [[UIImage alloc] initWithData:data];下面调用以下：

    if (data.length/1024 > 1024) {
               image = [self compressImageWith:image];
           }

当data大于1M的时候做压缩处理。革命尚未成功，还需要一步处理。在SDWebImageDownloaderOperation的connectionDidFinishLoading方法里面的：

   

      UIImage *image = [UIImage sd_imageWithData:self.imageData];

      //将等比压缩过的image在赋在转成data赋给self.imageData
      NSData *data = UIImageJPEGRepresentation(image, 1);
      self.imageData =  [NSMutableData dataWithData:data];

大工告成，我们来看一下效果吧：

<img src="http://mmbiz.qpic.cn/mmbiz/8RTSPr4mlykHge8gs1dCibkoTYXiatCCTMekY5MbfNibjW49ibygpHP440kOuxIVngeEXZLnYCUlSfIhW61x7T6OHQ/640" /></a>
​

果然问题得以解决。

更多iOS开发相关技术请关注iOS开发微信公众号 iOS开发 ：

	iOSDevTip
