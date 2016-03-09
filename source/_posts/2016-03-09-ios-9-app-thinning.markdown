---
layout: post
title: iOS 9 App Thinning
date: 2016-03-09 21:38:29 +0800
comments: true
categories: 推荐

---


App Thinning 是苹果在去年 WWDC, 也就是 iOS 9 中推出的为 App 瘦身的一个新的技术。 App Thinning 主要分为 3 个部分:

* App Slicing
* Bitcode
* On Demand Resources

##App Slicing

我们正常的应用中都会用到图片资源，为了适配 iPhone 6 Plus 以及 iPhone 6, 图片资源需要包含 @2x 的图片 和 @3x 的图片，而 Slicing 就是在这一步进行优化，Slicing 会根据用户在 App Store 下载的不同设备选择对应的资源。例如：当用户使用 iPhone 6 在 App Store 上下载 App 的时候，App Store 只会给予他 @2x 的图片，而不会像以前一样，不管三七二十一我把所有的资源都给你，根据苹果的描述，这大约能节约你 30% 到 50% 的大小。而你要做的事情，仅仅是使用 Xcode 7 IDE, 并且使用其中的 Assert Catalog 管理你的资源文件。

不过 Slicing 因为 iCloud 备份的问题, 知道 iOS 9.0.2 以及 Xcode 7.0.1 才可以被使用
##Bitcode

Bitcode 是苹果在你 App 被下载之前做的另外一件优化。Bitcode 是你的代码编译为最终可执行文件的中间的一层，当你的 App 以 Bitcode 的形式提交到 iTunes Connect 的时候，iTunes Connect 会自动为合适的机器编译出对应的代码，例如：对于 64 位机器的 iPhone 6 或者 iPad Air 2，Bitcode 会为其编译出适合 64 位机器可执行文件。

对于 iOS app 来说, Bitcode 不是必须的，但 Bitcode 编译选项为 Yes（这里应该只是一个过渡期，有些第三方库如果不在继续维护了需要尽快换掉）, 对于 tvOS 和 watchOS 的 app，Bitcode 的的选项必须为 Yes。
<!--more-->

##On Demand Resources

最后一个 App Thinnig 步骤是 On Demand Resources, On Demand Resources 在游戏的制作中比较常见，主要为了一些没用的资源在用到了才会去下载，比如，当我们设计一个游戏有 3 个关卡的时候，第一关的资源在 App 被下载的时候就已经下载好了，但是，出于对 App 大小的考虑，此时，第二关的资源是不需要下载的, 可以等到第一关结束以后，或者第一关进行到一半的时候后台进行下载。

On Demand Resources 中的资源包括了 图片，音效等等， 下面是一个 On Demand Resources 的 Demo, 语言的话，我们选用 Swift.

Demo 是这样的： 首页只有一个 Button，默认情况下 Assets 里面的图片 Group 没有被下载, 当点击 Button 以后，会先去看 Group 时候已经存储下载完毕, 如果没有下载，则会去触发一个下载请求，下载完成后，把图片显示到居中的位置。

	@IBAction func downloadBarButtonItemTouchUpInside(sender: UIButton) {
	    let tags = NSSet(array: ["Group"])
	    self.resourceRequest = NSBundleResourceRequest(tags: tags as! Set<String>)
	    self.resourceRequest?.conditionallyBeginAccessingResourcesWithCompletionHandler() {
	        resourceAvailable in
	
	        if resourceAvailable {
	            dispatch_async(dispatch_get_main_queue()) {
	                self.imageView.image = UIImage(named: "Group")
	            }
	        } else {
	
	            self.resourceRequest?.beginAccessingResourcesWithCompletionHandler() {
	                error in
	                if error == nil {
	                    dispatch_async(dispatch_get_main_queue()) {
	                        self.imageView.image = UIImage(named: "Group")
	                    }
	                } else {
	                    print(error)
	                }
	            }
	        }
	    }
	
	}

另外，我们可以通过 Xcode 中的 Disk 来查看 On Demand Resources 的情况

![1](http://blog.ggteach.com/img/app_thinning.jpg)

完整的 Demo 见 [这里](https://github.com/VioletHill/AppThinningDemo)

	
	
