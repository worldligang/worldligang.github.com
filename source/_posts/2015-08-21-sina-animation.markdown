---
layout: post
title: 一句代码添加新浪弹出框动画
date: 2015-08-21 11:16:07 +0800
comments: true
categories: 推荐
keywords: 弹出框动画, 新浪微博, 切换动画, iOS开发, 刚刚在线
description:  新浪的背景动画效果是有一个透明渐变的过程，并且最终渐变停止之后显示的是一个带有毛玻璃（半透明模糊）效果的视图。我的模仿思路是当准备要弹出动画的时候对整个视图进行截屏操作，将截屏后的图片进行毛玻璃效果渲染，然后在视图上加一个背景UIImageView来显示这张图片，然后通过alpha动画来由透明逐渐渐变出来。

---

新浪微博中的“加号按钮”点击后的弹出动画很有意思，每当一个人孤单寂寞冷的时候总会不停的点这个动画，终于忍不住自己撸了一个。废话不多说，直接上效果图：
![演示1](http://7xkdhe.com1.z0.glb.clouddn.com/sinaAnimation1.gif)

首先说一下通过这个动画制作过程给大家分享的技术问题：
1.背景的毛玻璃效果
2.弹性动画
3.动画进行时对用户交互的处理
4.`UIControl` 的 `event` 类型

<!--more-->

##一、背景的毛玻璃效果

对新浪微博中弹出动画背景的思路分析：

  新浪的背景动画效果是有一个透明渐变的过程，并且最终渐变停止之后显示的是一个带有毛玻璃（半透明模糊）效果的视图。我的模仿思路是当准备要弹出动画的时候对整个视图进行截屏操作，将截屏后的图片进行毛玻璃效果渲染，然后在视图上加一个背景 `UIImageView` 来显示这张图片，然后通过 `alpha` 动画来由透明逐渐渐变出来。

###1.对视图进行截屏：

        UIGraphics BeginImageContextWithOptions(size, NO, scale);
        [view.layer renderInContext:UIGraphicsGetCurrentContext()];
        UIImage * image = UIGraphicsGetImageFromCurrentImageContext();
        

###2.截屏后加上毛玻璃效果：

制作毛玻璃效果有三种选择，一是 `iOS8` 后推出的 `UIBlurEffect` 和 `UIVisualEffectView` 来直接显示一个带有毛玻璃效果的 `View` 不过这种方式并不能直接生成一张带有毛玻璃效果的图片，而且它的模糊程度设置方法非常有限只有那几个枚举类型，无法满足需求。


第二种是通过 `CoreImage` 添加滤镜的方式来实现毛玻璃效果，不过这种方式有个缺点如果滤镜使用频繁会对主线程产生影响，如果我不断频繁的重复动画效果就必须要做判断看滤镜是否正在起作用，否则会经常出现崩溃和内存泄漏问题。


第三种我们使用苹果13年 `WWDC` 上发布的官方 `sample` 一个 `UIImage` 的分类 `UIImage+ImageEffects.h` ,它不但可以制作毛玻璃效果图片，而且可以调整模糊程度和颜色渲染。下面给出代码：

        image = [image bhb_applyBlurWithRadius:15 
                                     tintColor:tintColor 
                         saturationDeltaFactor:1 
                                     maskImage:nil];//因为OC没有命名空间，避免你的程序中使用到了这个分类导致冲突，我加了前缀

最终的显示效果很不错，我将模糊程度尽量的调节到与新浪微博一致了，不过在这个过程中，我发现当我频繁的进行弹出操作时，内存会不断攀升如下图：
![演示3](http://7xkdhe.com1.z0.glb.clouddn.com/sinaAnimation3.png)
内存暴增的原因肯定是因为刚才的截图或者毛玻璃效果导致的，我们来用 `Instruments` 的 `Allocations` 来进行内存分析，找出元凶。
Xcode -> Product -> Profile -> Allocations， 开启之后我们来使用右下角的 `Mark generation`（内存快照功能，进一步了解请移步[这片文章][1]）
![演示4](http://7xkdhe.com1.z0.glb.clouddn.com/sinaAnimation4.png)
在弹出 `View` 和移除 `View` 的两个时间点加 `Mark generation`。
![演示5](http://7xkdhe.com1.z0.glb.clouddn.com/sinaAnimation5.png)
可以观察到，两个时间点竟然相差了4.31M，这是一个相当恐怖的数字，怪不得我点击几次弹出功能之后内存会暴增，让我们继续跟踪：
![演示6](http://7xkdhe.com1.z0.glb.clouddn.com/sinaAnimation6.png)
![演示7](http://7xkdhe.com1.z0.glb.clouddn.com/sinaAnimation7.png)
![演示8](http://7xkdhe.com1.z0.glb.clouddn.com/sinaAnimation8.png)
跟踪到最后，我们发现大部分未释放的内存来自于绘图和位图的创建，回想我们当初做的截图操作，图片上下文开启后并没有进行关闭操作，所以在程序不断截图的过程中开启了无数的图片上下文而且不会被释放，添加下面这句关键的代码就可以解决问题（NC的我竟然连这个都忘了加-.-）：

            UIGraphicsEndImageContext();

##二、弹性动画
新浪动画中，按钮弹出的动画为弹性效果，按钮到达最终位置后不会直接停止，而是做类似弹簧的一种阻尼运动，要实现这种动画也很简单 `iOS7` 后苹果非常给力的添加了 `spring` 弹性动画的快速创建方式：

        [UIView animateWithDuration:(NSTimeInterval) 
                              delay:(NSTimeInterval) 
             usingSpringWithDamping:(CGFloat) 
              initialSpringVelocity:(CGFloat) 
                            options:(UIViewAnimationOptions) 
                         animations:^{} 
                         completion:^(BOOL finished) {}];

通过对 `Damping` 阻力和 `Velocity` 初速度的设置可以实现弹性动画动画效果如下图：
![摘自MotionDesign](http://7xkdhe.com1.z0.glb.clouddn.com/threeballs.gif)

当然实现弹性动画还可以使用[Jonathan Willing][2]大大的 `JNWSpringAnimation` ,你可以像使用 `CABasicAnimation` 一样轻松的使用它，通过改变关键的3个属性 `Damping` 阻力， `stiffness` 硬直, `mass` 质量来改变弹性动画的效果代码如下：

        JNWSpringAnimation *scale = [JNWSpringAnimation
        animationWithKeyPath:@"transform.translation.x"];
        scale.damping = 7;
        scale.stiffness = 7;
        scale.mass = 1;

        scale.fromValue = @(0);
        scale.toValue = @(400);

        [redBall.layer addAnimation:scale forKey:scale.keyPath];
        redBall.transform = CGAffineTransformMakeTranslation(400, 0);

关键的三个属性对动画的影响如下图：
![摘自MotionDesign](http://7xkdhe.com1.z0.glb.clouddn.com/jnwdemo.gif)

为了减轻项目对第三方框架的依赖，我使用了 `iOS7` 原生的 `spring` 动画，如果你想要兼容以前版本，替换成 `JNWSpringAnimation` 即可。回到新浪动画的制作，动画是6个按钮按照顺序依次出现和消失，并且点击 `more` 按钮后可以向左平移到第二屏幕，并且在第二屏幕点击 `叉号` 按钮动画会加在第二屏幕的6个按钮上。
效果如图：
![演示2](http://7xkdhe.com1.z0.glb.clouddn.com/sinaAnimation2.gif)

对此我使用了一个 `UIScrollView` 来承载这些按钮，并声明2个数组用来保存所有的按钮和正在显示的按钮（在屏幕上，并且需要做动画）:

        @property (nonatomic,strong) NSMutableArray * visableArray;//屏幕显示的按钮数组
        @property (nonatomic,strong) NSMutableArray * itemsArray;//所有按钮的数组

这样我在给这些按钮加动画的时候就不会浪费性能，只把动画加在当前显示在屏幕的按钮上。动画依次按照一定的时间差来执行，解决的办法我是用的 `GCD` :

        [self.visableArray enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
            BHBCustomBtn * btn = obj;
            CGFloat x = btn.frame.origin.x;
            CGFloat y = btn.frame.origin.y;
            CGFloat width = btn.frame.size.width;
            CGFloat height = btn.frame.size.height;
            btn.frame = CGRectMake(x, [UIScreen mainScreen].bounds.size.height + y - self.frame.origin.y, width, height);
            btn.alpha = 0.0;
            dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(idx * 0.03 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
                [UIView animateWithDuration:0.35 delay:0 usingSpringWithDamping:0.85 initialSpringVelocity:25 options:UIViewAnimationOptionCurveEaseIn animations:^{
                    btn.alpha = 1;
                    btn.frame = CGRectMake(x, y, width, height);
                } completion:^(BOOL finished) {
                    if ([btn isEqual:[self.visableArray lastObject]]) {
                        self.superview.superview.userInteractionEnabled = YES;
                    }
                }];
            });
        }];

这样按钮就会依次以弹性动画的形式弹动出来了，是不是很简单，对于动画结束后所作的处理我将在下一节中说明。

##三、动画进行时对用户交互的处理

动画过程中，处理用户交互的问题相当关键，视图动画中默认自动停止响应用户交互，因此当按钮进行弹性动画时，触摸它并不会生成任何事件。
但是当你触摸加号按钮的时候，会再次进行弹出框动画，这时就会弹出两个弹出框，这是我们不希望看到的，我们可以将加号按钮的 `enable` 属性设置为 `YES` 但是这样做我们要在封装的视图内部获取外部的加号按钮，这一点违背了封装性原则，并不是一个好的设计。所以我在所有按钮的弹性动画开始时，设置：

        self.superview.superview.userInteractionEnabled = NO;

注意视图的层级关系，我所设计的 `BHBPopView` 内部装着一个 `UIScrollView`，而它上面放着这些按钮，所以你要找到父视图的父视图才能够统一屏蔽用户的交互行为，当所有的按钮弹性动画结束时，也就是 `visableArray` 数组最后一个按钮动画结束时，我们恢复用户的交互：

        self.superview.superview.userInteractionEnabled = YES;

这样当动画进行过程中，屏蔽了用户的交互，避免发生一些意外的情况。

##四、`UIControl` 的 `event` 类型

注意到新浪动画里面按钮有一个放大效果，并且当你手指放上去的时候放大，手指稍微挪动，便恢复原始大小。让我们先来看一下动画：

![演示9](http://7xkdhe.com1.z0.glb.clouddn.com/sinaAnimation9.gif)

要实现这个效果只需要做一个形变动画就可以了，关键是我们如何控制它放大和恢复大小。
思路如下：按钮是继承自 `UIControl` ，`UIControl` 有不同的事件状态：

        
        UIControlEventTouchDown           = 1 <<  0,      // 手指落在按钮的一瞬间触发
        UIControlEventTouchDownRepeat     = 1 <<  1,      // 多点触碰的时候，当第二根以上的手指触摸瞬间出发
        UIControlEventTouchDragInside     = 1 <<  2,      // 手指在视图范围内拖动触发
        UIControlEventTouchDragOutside    = 1 <<  3,      // 手指在视图范围外拖动触发
        UIControlEventTouchDragEnter      = 1 <<  4,      // 手指从视图外拖动到视图内时触发
        UIControlEventTouchDragExit       = 1 <<  5,      // 手指从视图内部拖动到视图外时触发
        UIControlEventTouchUpInside       = 1 <<  6,      // 手指在视图内部抬起时触发
        UIControlEventTouchUpOutside      = 1 <<  7,      // 手指在视图外部抬起时触发
        UIControlEventTouchCancel         = 1 <<  8,      // 取消事件，放上了太多手指或者被上锁或者电话呼叫打断。

        UIControlEventValueChanged        = 1 << 12,      // 当视图的值发生改变时，发送通知。

        UIControlEventEditingDidBegin     = 1 << 16,     // UITextField
        UIControlEventEditingChanged      = 1 << 17,
        UIControlEventEditingDidEnd       = 1 << 18,
        UIControlEventEditingDidEndOnExit = 1 << 19,     // 'return key' ending editing

        UIControlEventAllTouchEvents      = 0x00000FFF,  // for touch events
        UIControlEventAllEditingEvents    = 0x000F0000,  // for UITextField
        UIControlEventApplicationReserved = 0x0F000000,  // range available for application use
        UIControlEventSystemReserved      = 0xF0000000,  // range reserved for internal framework use
        UIControlEventAllEvents           = 0xFFFFFFFF



我们所用到的事件是 `TouchDown` 和 `DragInside`，手指放上去触发 `TouchDown` 放大视图，在视图内部移动 `DragInside` 时恢复视图，注意按钮的作用范围是整个矩形区域包含了图片和文字，当你的手指移出图片的时候并非一定会移出按钮作用范围，所以依然会触发 `TouchUpInsite` 事件，这时候我们需要做一个属性来记录用户拖拽之后取消按钮的 `TouchUpInsite` 执行。

        @property (nonatomic,assign) BOOL btnCanceled;

这样我们就可以实现动画效果了，具体代码如下：

        //处理按钮有效的点击事件，当前按钮放大消失，其他按钮缩小消失，回调点击事件
        [btn addTarget:self action:@selector(didClickBtn:) forControlEvents:UIControlEventTouchUpInside];
        //处理手指按下事件，放大按钮
        [btn addTarget:self action:@selector(didTouchBtn:) forControlEvents:UIControlEventTouchDown];
        //处理手指拖动事件，恢复按钮大小
        [btn addTarget:self action:@selector(didCancelBtn:) forControlEvents:UIControlEventTouchDragInside];

##总结：
制作类似新浪微博这种弹出框动画，我的思路是先分析逻辑，这些特效都由哪些组成，毛玻璃背景，加顶部一个 `logo` ，加中间 `UIScrollView`和上面的很多按钮,加底部工具条。研究透彻动画的执行顺序，动画执行结果有哪些分支。然后针对特效中的难点，比如毛玻璃，按钮弹性动画等等进行逐一研究攻破，最后将这些组件整合在一起变成一个好玩的动画，最后不要忘了动画的内存和性能测试。这次我模仿的新浪微博动画弹性效果并不是太理想，比起新浪原生来说不是特别一致，也希望有兴趣的你来给我一些建议优化它。最终在你的项目中加入我的弹出框动画真的只需要一句话哦：

        /**
        *  直接显示一个popView在某个view上
        *
        *  @param view       父view
        *  @param imageArray 图标数组
        *  @param titles     标题数组
        *  @param block      回调
        *  @return pop视图
        */
        + (BHB_INSTANCETYPE)showToView:(UIView *)view andImages:(NSArray *)imageArray andTitles:(NSArray *)titles andSelectBlock:(DidSelectItemBlock)block;
        /**
        *  如果显示一个带more功能的，请使用此方法
        *
        *  @param view  父view
        *  @param array BHBItem类型的集合
        *  @param block 回调
        *  @return pop视图
        */
        + (BHB_INSTANCETYPE)showToView:(UIView *)view withItems:(NSArray *)array andSelectBlock:(DidSelectItemBlock)block;

hexo出点问题修复到下半夜啊（升级到3.0太蛋疼了），现在脑子晕晕的，明天还要去新公司入职，动画中还有很多细节我不能一一分享了，欢迎大家来搞我的[Demo](https://github.com/bb-coder/BHBPopView)

good luck!

本文由[程序员头条](http://www.90159.com/)管理员毕洪博原创文章，转载务必注明出处。

版权声明：我已将本文在微信公众平台的发表权「独家代理」给 iOS开发（ iOSDevTip ） 微信公众号。扫下方二维码即可关注「iOS 开发」：

![iOSDevTip](http://images.90159.com/icon/iOSDevTip.jpg)

原文链接：http://bihongbo.com/2015/08/19/sinaAnimation/
