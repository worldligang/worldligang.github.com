---
layout: post
title: "iOS开发之保存照片到自己创建的相簿"
date: 2015-08-04 22:41:08 +0800
comments: true
categories: iOS开发
keywords: 保存照片, 系统相册, Photo Album, iOS开发, 刚刚在线
description: 保存照片还可以用ALAssetsLibrary，ALAssetsLibrary提供了我们对iOS设备中的相片、视频的访问，是连接应用程序和相册之间访问的一个桥梁。接下来，我们来详细讲解一下关于系统相册权限获取、保存照片、创建自己的相簿等等功能。

---

在[刚刚在线](http://www.superqq.com/)上一篇文章[iOS开发之保存照片到系统相册（Photo Album）](http://www.superqq.com/blog/2015/08/03/ioskai-fa-zhi-bao-cun-zhao-pian-dao-xi-tong-xiang-ce-%28photo-album%29/)，我们讲到了如何保持照片系统相册。还有其他保存的方法吗？

保存照片还可以用`ALAssetsLibrary`，`ALAssetsLibrary`提供了我们对iOS设备中的相片、视频的访问，是连接应用程序和相册之间访问的一个桥梁。

接下来，我们来详细讲解一下关于系统相册权限获取、保存照片、创建自己的相簿等等功能。


<!--more-->

##创建自己的相簿

这也是一种比较创建的作法，创建自己的相簿，然后把照片或者视频保存到自己的相簿中。相关代码如下：

	  ALAssetsLibrary *library = [[ALAssetsLibrary alloc] init];
    [library addAssetsGroupAlbumWithName:@"test" resultBlock:^(ALAssetsGroup *group) {
        
        //创建相簿成功
        
    } failureBlock:^(NSError *error) {
        //失败
    }];

##保存照片

这个方法也是将照片保存到系统相簿里面，不是保存到自己创建的相簿里面。代码如下：

	 ALAssetsLibrary *library = [[ALAssetsLibrary alloc]init];
    [library writeImageToSavedPhotosAlbum:image.CGImage orientation:(ALAssetOrientation)image.imageOrientation completionBlock:^(NSURL *asSetUrl,NSError *error){
        if (error) {
           //失败
        }else{
            UIAlertView *alert = [[UIAlertView alloc]initWithTitle:@"存储成功"
                                                           message:nil
                                                          delegate:nil
                                                 cancelButtonTitle:@"确定"
                                                 otherButtonTitles:nil, nil];
            [alert show];
            
        }
    }];


##获取权限

在保存照片之前，如果用户关闭相册权限，这个时候是保存失败的。如果你不做任何处理，用户是不会知道自己保存失败了。所以，我们可以在保存照片之前，做出相应的提示。如何获取这个权限呢？一般有两种方法：

1. 创建相簿失败

2. 保存照片失败

在上面两个方法**创建自己的相簿**和**保存照片**的失败结果里，我们可以弹出获取照片权限失败的提示。我们拿第一个创建相簿失败来举例：


	ALAssetsLibrary *library = [[ALAssetsLibrary alloc] init];
    [library addAssetsGroupAlbumWithName:@"test" resultBlock:^(ALAssetsGroup *group) 	{
  	           
          //创建相簿成功

    } failureBlock:^(NSError *error) {
      	UIAlertView *alert = [[UIAlertView alloc]initWithTitle:@"存储失败"
                                                           message:@"请打开 设置-隐私-照片 来进行设置"
                                                          delegate:nil
                                                 cancelButtonTitle:@"确定"
                                                 otherButtonTitles:nil, nil];
        [alert show];
    }];

在保存照片失败的结果里，我们也可以弹出相应的提示，让用户打开应用程序的相册权限。

##保存照片到自己的相簿    

下面这段代码是在大谷歌里面找到的，亲自测试过是可以用的，整理如下：      
        
	#pragma mark - 创建相册
	- (void)createAlbum
	{
	    ALAssetsLibrary *assetsLibrary = [[ALAssetsLibrary alloc] init];
	    NSMutableArray *groups=[[NSMutableArray alloc]init];
	    ALAssetsLibraryGroupsEnumerationResultsBlock listGroupBlock = ^(ALAssetsGroup *group, BOOL *stop)
	    {
	        if (group)
	        {
	            [groups addObject:group];
	        }
	        
	        else
	        {
	            BOOL haveHDRGroup = NO;
	            
	            for (ALAssetsGroup *gp in groups)
	            {
	                NSString *name =[gp valueForProperty:ALAssetsGroupPropertyName];
	                
	                if ([name isEqualToString:@"iOSDevTip"])
	                {
	                    haveHDRGroup = YES;
	                }
	            }
	            
	            if (!haveHDRGroup)
	            {
	                //do add a group named "XXXX"
	                [assetsLibrary addAssetsGroupAlbumWithName:@"iOSDevTip"
	                                               resultBlock:^(ALAssetsGroup *group)
	                 {
	                     [groups addObject:group];
	                     
	                 }
	                                              failureBlock:nil];
	                haveHDRGroup = YES;
	            }
	        }
	        
	    };
	    //创建相簿
	    [assetsLibrary enumerateGroupsWithTypes:ALAssetsGroupAlbum usingBlock:listGroupBlock failureBlock:nil];
	    
	    [self saveToAlbumWithMetadata:nil imageData:UIImagePNGRepresentation(self.image) customAlbumName:@"iOSDevTip" completionBlock:^
	     {
	         //这里可以创建添加成功的方法
	         
	     }
	                     failureBlock:^(NSError *error)
	     {
	         //处理添加失败的方法显示alert让它回到主线程执行，不然那个框框死活不肯弹出来
	         dispatch_async(dispatch_get_main_queue(), ^{
	             
	             //添加失败一般是由用户不允许应用访问相册造成的，这边可以取出这种情况加以判断一下
	             if([error.localizedDescription rangeOfString:@"User denied access"].location != NSNotFound ||[error.localizedDescription rangeOfString:@"用户拒绝访问"].location!=NSNotFound){
	                 UIAlertView *alert=[[UIAlertView alloc]initWithTitle:error.localizedDescription message:error.localizedFailureReason delegate:nil cancelButtonTitle:NSLocalizedString(@"ok", nil) otherButtonTitles: nil];
	                 
	                 [alert show];
	             }
	         });
	     }];
	}
	
	- (void)saveToAlbumWithMetadata:(NSDictionary *)metadata
	                      imageData:(NSData *)imageData
	                customAlbumName:(NSString *)customAlbumName
	                completionBlock:(void (^)(void))completionBlock
	                   failureBlock:(void (^)(NSError *error))failureBlock
	{
	    
	    ALAssetsLibrary *assetsLibrary = [[ALAssetsLibrary alloc] init];
	    __weak ALAssetsLibrary *weakSelf = assetsLibrary;
	    void (^AddAsset)(ALAssetsLibrary *, NSURL *) = ^(ALAssetsLibrary *assetsLibrary, NSURL *assetURL) {
	        [assetsLibrary assetForURL:assetURL resultBlock:^(ALAsset *asset) {
	            [assetsLibrary enumerateGroupsWithTypes:ALAssetsGroupAll usingBlock:^(ALAssetsGroup *group, BOOL *stop) {
	                
	                if ([[group valueForProperty:ALAssetsGroupPropertyName] isEqualToString:customAlbumName]) {
	                    [group addAsset:asset];
	                    if (completionBlock) {
	                        completionBlock();
	                    }
	                }
	            } failureBlock:^(NSError *error) {
	                if (failureBlock) {
	                    failureBlock(error);
	                }
	            }];
	        } failureBlock:^(NSError *error) {
	            if (failureBlock) {
	                failureBlock(error);
	            }
	        }];
	    };
	    [assetsLibrary writeImageDataToSavedPhotosAlbum:imageData metadata:metadata completionBlock:^(NSURL *assetURL, NSError *error) {
	        if (customAlbumName) {
	            [assetsLibrary addAssetsGroupAlbumWithName:customAlbumName resultBlock:^(ALAssetsGroup *group) {
	                if (group) {
	                    [weakSelf assetForURL:assetURL resultBlock:^(ALAsset *asset) {
	                        [group addAsset:asset];
	                        if (completionBlock) {
	                            completionBlock();
	                        }
	                    } failureBlock:^(NSError *error) {
	                        if (failureBlock) {
	                            failureBlock(error);
	                        }
	                    }];
	                } else {
	                    AddAsset(weakSelf, assetURL);
	                }
	            } failureBlock:^(NSError *error) {
	                AddAsset(weakSelf, assetURL);
	            }];
	        } else {
	            if (completionBlock) {
	                completionBlock();
	            }
	        }
	    }];
	}
	

    
##ALAssetsLibrary+CustomPhotoAlbum保存照片

`github`上有一个项目[ALAssetsLibrary+CustomPhotoAlbum](https://github.com/Kjuly/ALAssetsLibrary-CustomPhotoAlbum)，讲保存照片做了很好的封装。使用之前记得先导入头文件：

	#import "ALAssetsLibrary+CustomPhotoAlbum.h"


保存照片到自己的相簿，直接调用：

	ALAssetsLibrary *library = [[ALAssetsLibrary alloc] init];
    [library saveImage:self.image toAlbum:@"gang" completion:^(NSURL *assetURL, NSError *error) {
        if (!error) {
            
        }
    } failure:^(NSError *error) {
       
    }];

`ALAssetsLibrary+CustomPhotoAlbum`对保存视频的封装也是非常好的。我现在项目中用的也是这个第三方来保存照片和视频。因为不错，所以推荐给大家使用。
    
