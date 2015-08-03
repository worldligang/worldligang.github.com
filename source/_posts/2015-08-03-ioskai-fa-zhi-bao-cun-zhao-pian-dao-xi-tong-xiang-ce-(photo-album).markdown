---
layout: post
title: "iOS开发之保存照片到系统相册（Photo Album）"
date: 2015-08-03 21:43:34 +0800
comments: true
categories: iOS开发
keywords: 保存照片, 系统相册, Photo Album, iOS开发, 刚刚在线
description: 保存照片到系统相册这个功能很多社交类的APP都有的，今天我们简单讲解一下，如何将图片保存到系统相册（Photo Album）。

---


保存照片到系统相册这个功能很多社交类的APP都有的，今天我们简单讲解一下，如何将图片保存到系统相册（Photo Album）。

##创建UIImageView

创建`UIImageView`是为了将照片展示出来，我们是要把`UIImage`保存到系统相册（Photo Album）:

	#define SCREEN [UIScreen mainScreen].bounds.size

	self.image = [UIImage imageNamed:@"iOSDevTip"];
    UIImageView *imageView = [[UIImageView alloc] initWithFrame:CGRectMake((SCREEN.width - 300) / 2, 70, 300, 150)];
    imageView.image = self.image;
    [self.view addSubview:imageView];
    
##创建UIButton

创建`UIButton`并绑定`actionClick:`事件:

 	UIButton *button = [[UIButton alloc] init];
    button.frame = CGRectMake( 100, 300, SCREEN.width - 200, 40);
    [button addTarget:self action:@selector(actionClick:) forControlEvents:UIControlEventTouchUpInside];
    [button setTitleColor:[UIColor blueColor] forState:UIControlStateNormal];
    [button setBackgroundColor:[UIColor orangeColor]];
    [button setTitle:@"SavePhoto" forState:UIControlStateNormal];
    [self.view addSubview:button];
    
    
    - (void)actionClick:(UIButton *)button
	{
		
	}
    
##保存照片到系统相册（Photo Album）

在`actionClick:`方法里调用：

	UIImageWriteToSavedPhotosAlbum(self.image, self, @selector(image:didFinishSavingWithError:contextInfo:), NULL);
   
这个时候，我们想知道保存是否成功，所以需要制定毁掉方法   
    
	// 指定回调方法
	- (void)image:(UIImage *)image didFinishSavingWithError:(NSError *)error contextInfo:(void *)contextInfo
	{
	    if(!error){
	        NSLog(@"save success");
	    }else{
	        NSLog(@"save failed");
	    }
	}

在这个方法里，我们就知道照片是否保存成功。然后，根据需求来刷新UI线程。用户第一次使用APP的时候，点击`SavePhoto`按钮的时候，系统会询问权限：

![photo](http://7xkkk9.com1.z0.glb.clouddn.com/46869740F32868C1F9769ED5FED1C7D0.png)

Demo地址:[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo) 下一篇文章，将会讲如何创建自己的相册并保存照片。
	
