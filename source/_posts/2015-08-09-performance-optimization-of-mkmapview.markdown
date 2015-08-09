---
layout: post
title: "performance-optimization-of-MKMapView"
date: 2015-08-09 13:18:21 +0800
comments: true
categories: 推荐
keywords: 招聘, iOS面试题, iOS面试, iOS开发, 刚刚在线
description: 首先看下我是怎么实现这个annotationView的,由于这个annotationsView是异形的(也就是无法通过设置圆角直接得到)而且里面的图片还因用户而异.所以解决方案就是使用layer.mask来进行遮罩.

---

##前言
最近做的项目主要是LBS这块 主打成员定位功能 我们的UI设计是这样的

![Pic-MK-1](http://7xkkjz.com1.z0.glb.clouddn.com/MK-1.png)

乍一看上去是挺好挺美观的 不同的人会显示不同的头像 可是当人扎堆的时候 问题就来了

![Pic-MK-2](http://7xkkjz.com1.z0.glb.clouddn.com/MK-2.png)

当人多的时候(例如上图所示) 地图滑动起来就能感觉到明显顿卡 那种不流畅感能折磨死人 所以 自然我们要解决这个问题(等等 先不要吐槽为什么不用地图聚合 因为这已经是地图放到最大了 聚合不适合这次的问题讨论)

##分析
首先看下我是怎么实现这个annotationView的 由于这个annotationsView是异形的(也就是无法通过设置圆角直接得到) 而且里面的图片还因用户而异 所以解决方案就是使用layer.mask来进行遮罩 代码如下

	@implementation MMAnnotationView
	
	- (instancetype)initWithAnnotation:(id<MKAnnotation>)annotation reuseIdentifier:(NSString *)reuseIdentifier
	{
	    self = [super initWithAnnotation:annotation reuseIdentifier:reuseIdentifier];
	    if ( self )
	    {
	        self.frame = CGRectMake(0, 0, TRACK_ANNOTATION_SIZE.width, TRACK_ANNOTATION_SIZE.height);
	        self.centerOffset = CGPointMake(0, -(TRACK_ANNOTATION_SIZE.height-3)/2);
	        self.canShowCallout = NO;
	
	        self.avatarView = [[UIImageView alloc] initWithFrame:self.bounds];
	        [self addSubview:self.avatarView];
	        self.avatarView.contentMode = UIViewContentModeScaleAspectFill;
	
	        CAShapeLayer *shapelayer = [CAShapeLayer layer];
	        shapelayer.frame = self.bounds;
	        shapelayer.path = self.framePath.CGPath;
	        self.avatarView.layer.mask = shapelayer;
	
	        self.layer.shadowPath = self.framePath.CGPath;
	        self.layer.shadowRadius = 1.0f;
	        self.layer.shadowColor = [UIColor colorWithHex:0x666666FF].CGColor;
	        self.layer.shadowOpacity = 1.0f;
	        self.layer.shadowOffset = CGSizeMake(0, 0);
	        self.layer.masksToBounds = NO;
	    }
	    return self;
	}
	
	//mask路径
	- (UIBezierPath *)framePath
	{
	    if ( !_framePath )
	    {
	        CGFloat arrowWidth = 14;
	
	        CGMutablePathRef path = CGPathCreateMutable();
	
	        CGRect rectangle = CGRectInset(CGRectMake(0, 0, CGRectGetWidth(self.bounds), CGRectGetWidth(self.bounds)), 3,3);
	
	        CGPoint p[3] = {
	        {CGRectGetMidX(self.bounds)-arrowWidth/2, CGRectGetWidth(self.bounds)-6},
	        {CGRectGetMidX(self.bounds)+arrowWidth/2, CGRectGetWidth(self.bounds)-6},
	        {CGRectGetMidX(self.bounds), CGRectGetHeight(self.bounds)-4}
	        };
	
	        CGPathAddRoundedRect(path, NULL, rectangle, 5, 5);
	        CGPathAddLines(path, NULL, p, 3);
	
	        CGPathCloseSubpath(path);
	
	        _framePath = [UIBezierPath bezierPathWithCGPath:path];
	
	        CGPathRelease(path);
	    }
	
	    return _framePath;
	}
	
我用代码生成了形状路径 并以此生成了layer的mask和shadowPath
使用时 只要直接用SDWebImage设置头像就行了


	[annotationView.avatarView sd_setImageWithURL:[NSURL URLWithString:avatarURL] placeholderImage:placeHolderImage];
	
接下来用工具分析一下问题出来哪 分析性能当然是选择Instrments(用法在这里就不做介绍了) 打开Core Animation 然后运行程序 滑动地图 可以看到性能分析如下

![Pic-MK-3](http://7xkkjz.com1.z0.glb.clouddn.com/MK-3.jpg)

原来平均帧数只有不到30帧 这离我们的目标60帧差得实在太远

再使用Debug Option来深入分析一下

![Pic-MK-4](http://7xkkjz.com1.z0.glb.clouddn.com/MK-4.png)

由于MKMapView的原因 这里我们主要关心这几个选项

* Color Blended Layers
* Color Misaligned Images
* Color Offscreen-Rendered Yellow

分别打开这几个选项 结果如下

![Pic-MK-5](http://7xkkjz.com1.z0.glb.clouddn.com/MK-5.png)

可以看到

* Color Blended Layers没有问题 不过这也是正常的 由于使用了mask 没有透明的地方
* Color Misaligned Images除了默认头像外全中 这是因为服务器上的图片大小跟显示的大小不一致 导致缩放 而默认头像则是一致的 所以没问题
* Color Offscreen-Rendered Yellow全中 由于使用了mask 导致大量的离屏渲染 这也是性能下降的主要原因

##解决

问题的原因找到了 那么接下来该如何解决呢?

* 首先mask是肯定不能用了
* 其次下载下来的图片我们要预处理成实际大小
那么 直接把下载下来的图片合成为我们要显示的最终结果不就ok了吗? 试试看

	
		- (void)loadAnnotationImageWithURL:(NSString*)url imageView:(UIImageView*)imageView
		{
		    //将合成后的图片缓存起来
		    NSString *annoImageURL = url;
		    NSString *annoImageCacheURL = [annoImageURL stringByAppendingString:@"cache"];
		
		    UIImage *cacheImage = [[SDImageCache sharedImageCache] imageFromDiskCacheForKey:annoImageCacheURL];
		    if ( cacheImage )
		    {
		        //LLLog(@"hit cache");
		        imageView.image = cacheImage;
		    }
		    else
		    {
		        //LLLog(@"no cache");
		        [imageView sd_setImageWithURL:[NSURL URLWithString:annoImageURL]
		        placeholderImage:placeHolderImage
		        completed:^(UIImage *image, NSError *error, SDImageCacheType cacheType, NSURL *imageURL) {
		        if (!error)
		        {
		            UIImage *annoImage = [image annotationImage];
		            imageView.image = annoImage;
		
		            [[SDImageCache sharedImageCache] storeImage:annoImage forKey:annoImageCacheURL];
		            }
		        }];
		    }
		}
		
		@implementation UIImage (LJC)
		- (UIImage*) annotationImage
		{
		    static UIView *snapshotView = nil;
		    static UIImageView *imageView = nil;
		
		    if ( !snapshotView )
		    {
		        snapshotView = [UIView new];
		        snapshotView.frame = CGRectMake(0, 0, TRACK_ANNOTATION_SIZE.width, TRACK_ANNOTATION_SIZE.height);
		
		        imageView = [UIImageView new];
		        [snapshotView addSubview:imageView];
		        imageView.clipsToBounds = YES;
		        imageView.frame = snapshotView.bounds;
		        imageView.contentMode = UIViewContentModeScaleAspectFill;
		
		        CGFloat arrowWidth = 14;
		
		        CGMutablePathRef path = CGPathCreateMutable();
		
		        CGRect rectangle = CGRectInset(CGRectMake(0, 0, CGRectGetWidth(imageView.bounds), CGRectGetWidth(imageView.bounds)), 3,3);
		
		        CGPoint p[3] = {
		            {CGRectGetMidX(imageView.bounds)-arrowWidth/2, CGRectGetWidth(imageView.bounds)-6},
		            {CGRectGetMidX(imageView.bounds)+arrowWidth/2, CGRectGetWidth(imageView.bounds)-6},
		            {CGRectGetMidX(imageView.bounds), CGRectGetHeight(imageView.bounds)-4}
		        };
		
		        CGPathAddRoundedRect(path, NULL, rectangle, 5, 5);
		        CGPathAddLines(path, NULL, p, 3);
		
		        CGPathCloseSubpath(path);
		
		        CAShapeLayer *shapelayer = [CAShapeLayer layer];
		        shapelayer.frame = imageView.bounds;
		        shapelayer.path = path;
		
		        imageView.layer.mask = shapelayer;
		
		        snapshotView.layer.shadowPath = path;
		        snapshotView.layer.shadowRadius = 1.0f;
		        snapshotView.layer.shadowColor = [UIColor colorWithHex:0x666666FF].CGColor;
		        snapshotView.layer.shadowOpacity = 1.0f;
		        snapshotView.layer.shadowOffset = CGSizeMake(0, 0);
		
		        CGPathRelease(path);
		    }
		
		    imageView.image = self;
		
		    UIGraphicsBeginImageContextWithOptions(TRACK_ANNOTATION_SIZE, NO, 0);
		
		    [snapshotView.layer renderInContext:UIGraphicsGetCurrentContext()];
		
		    UIImage *copied = UIGraphicsGetImageFromCurrentImageContext();
		    UIGraphicsEndImageContext();
		
		    return copied;
		}
		@end
		
然后使用的时候 只要简单的如下调用就OK了


	[self loadAnnotationImageWithURL:avatarURL imageView:annotationView.avatarView];
	
看看修改之后的Instruments表现如何

![Pic-MK-6](http://7xkkjz.com1.z0.glb.clouddn.com/MK-6.png)

* Color Blended Layers全中 这也是无可避免的 因为显示的就是一张带透明度的图 但是由于地图的特殊性(头像的位置变化间隔较长 所以不会经常引发合成 也没有动画) 所以这里也不是问题
* Color Misaligned Images没问题了 因为头像已被缩放成了相同大小
* Color Offscreen-Rendered Yellow没问题了 因为只是简单的显示了一张图片 而并没有需要离屏渲染的东西了

再来看下帧数情况

![Pic-MK-7](http://7xkkjz.com1.z0.glb.clouddn.com/MK-7.jpg)

Oh-Yeah~ 不光帧数达到了我们的目标60帧(由于还有业务逻辑线程在后台跑 所以没有那么的稳定) 就连平均运行耗时都下降了不少 就算地图上再多显示几十个人 也不成问题了