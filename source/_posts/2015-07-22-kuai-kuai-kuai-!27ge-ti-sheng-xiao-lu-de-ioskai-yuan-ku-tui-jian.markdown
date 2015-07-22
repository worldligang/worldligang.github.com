---
layout: post
title: "快快快！27个提升效率的iOS开源库推荐"
date: 2015-07-22 13:11:49 +0800
comments: true
categories: 推荐
keywords: iOS开源, 开源库, iOS开源库, iOS开发, 刚刚在线
description: 我热爱开源，更喜爱那些花费宝贵的业余时间来创造奇迹的开发者们，感谢他们将自己辛苦劳动的成果无偿分享给大家。开源作者和贡献者们，你们太了不起了，感谢你们所付出的一切。

---

>CSDN移动将持续为您优选移动开发的精华内容，共同探讨移动开发的技术热点话题，涵盖移动应用、开发工具、移动游戏及引擎、智能硬件、物联网等方方面面。如果您想投稿、参与内容翻译工作，或寻求近匠报道，请发送邮件至tangxy#csdn.net（请把#改成@）。

我热爱开源，更喜爱那些花费宝贵的业余时间来创造奇迹的开发者们，感谢他们将自己辛苦劳动的成果无偿分享给大家。开源作者和贡献者们，你们太了不起了，感谢你们所付出的一切。

鉴于我是收集不全会死星人，这里我根据个人喜好选取了一些iOS的开源库。这些项目的顺序完全随机，每个都棒到爆。绝大多数支持CocoaPods，所以将它们添加到你的Xcode项目中只是小事一桩。

在本文末尾你可以找到一个长文慎入版本——一个只包含题目与项目链接的简单列表。如果你觉得这篇文章有用的话，请把它分享给其他做iOS开发的小伙伴们，好酒也怕巷子深。

###1. <a href="https://github.com/dzenbot/DZNEmptyDataSet" target="_blank" title=“DZNEmptyDataSet”>DZNEmptyDataSet</a>

DZNEmptyDataSet算是一个很标准的iOS内建方式，适合用来处理空的table view和collection view。默认情况下，如果你的table view为空，屏幕就为空，这样一来用户体验并不理想。

![1](http://img.ptcms.csdn.net/article/201507/21/55adfe02a4a2f_middle.jpg)

使用这个库，你只需遵守一些协议，iOS就会自动将collection view处理完善，并将用户消息以合适美观的方式显示出来。每个iOS项目都可以自动处理，不用再一一费神。

###2. <a href="https://github.com/jivesoftware/PDTSimpleCalendar" target="_blank" title=“PDTSimpleCalendar”>PDTSimpleCalendar</a>

你的应用需要一个简单美观且正常运行的日历组件吗？现在有了——PDTSimpleCalendar也许可以算是iOS最棒的日历组件了。你可以在各个方面对它进行定制，无论是运行逻辑还是外观方面。

![2](http://img.ptcms.csdn.net/article/201507/21/55adff269e028_middle.jpg)

###3. <a href="https://github.com/magicalpanda/MagicalRecord" target="_blank" title=“MagicalRecord”>MagicalRecord</a>


他们都说：Core Data简洁易用。他们又说：很棒很好用噢。哈？真的吗，苹果？添加到各个项目中的大量样板数据完全不符合简洁而易用的标准。更别提添加、移除和更新那一大堆的entities、保存context、按不同环境创建不同的Core Data堆栈等等。当然，我很喜欢Core Data，但是苹果真的可以把它更好地简化一下——使用MagicalRecord方式。

![3](http://img.ptcms.csdn.net/article/201507/21/55ae0130e5bb0_middle.jpg)

MagicalRecord就像是给Core Data提供了一层外包装，隐藏掉所有不相关的东西。如果你曾经使用过active record模式（比如Ruby on Rails），你就知道了。倾情推荐，在应用中使用Core Data的童鞋们可一定要试试。

###4. <a href="https://github.com/ViccAlexander/Chameleon" target="_blank" title=“Chameleon”>Chameleon</a>

如果你已经看到这里了，我猜你更有可能是个程序猿，而不是设计狮。这里有个东东很适合你。

![4](http://img.ptcms.csdn.net/article/201507/21/55ae02035ca71_middle.jpg)

Chameleon是一个iOS的色彩框架。它运用现代化flat color将UIColor扩展地非常美观。我们还可以通过它运用自定义颜色创建调色板。它还有很多功用，请浏览readme。如果你想要应用美观的话，一定要把这个库加到项目里。 

![4-1](http://img.ptcms.csdn.net/article/201507/21/55ae0212f22c6_middle.jpg)

###5. <a href="https://github.com/Alamofire/Alamofire" target="_blank" title=“Alamofire”>Alamofire</a>

Alamofire是一个简洁的网络库，用Swift语言编写。你是否曾经使用过AFNetworking呢？Alamofire是它的小弟。更年轻更时尚，当然（AFNetworking是用Objective-C编写的）。

![5](http://img.ptcms.csdn.net/article/201507/21/55ae024c07d47_middle.jpg)


如果你需要进行网络的相关工作，诸如下载、上传与获取JSONs等等的话，Alamofire正是你所需要的。GitHub上8000人次推荐，一定不会错。 


###6. <a href="https://github.com/raulriera/TextFieldEffects" target="_blank" title=“TextFieldEffects”>TextFieldEffects</a>

你不觉得标准的UITextField有些枯燥么？我也这样想——来认识一下TextFieldEffects吧！废话不多说，只要看几个例子：


![6](http://img.ptcms.csdn.net/article/201507/21/55ae048484702.jpg)

是啊，都是些简单的dropin控制器。你甚至可以在storyboard中使用IBDesignables。

不幸地是：这个库不支持CocoaPods（如果你来自未来，而这一情况什么时候会改变的话，一定要在Twitter上告诉我），但是它支持Carthage。你只管从GitHub上下载项目，把它放入你的workspace就行了。

###7. <a href="https://github.com/BradLarson/GPUImage" target="_blank" title=“GPUImage”>GPUImage</a>

你曾经写过照相机应用吗？如果没有的话，很快你一定会遇到这个库的。

![7](http://img.ptcms.csdn.net/article/201507/21/55ae04fe94fc8.jpg)

GPUImage为我们提供了一个GPU加速的照相机效果（同时支持照片与视频），而且处理速度飞快。在App Store中，使用这个库的应用数以百计。我有一个应用也用到了GPUImage。 它在GitHub上获得了8869个star，而且还在增长。

![7-1](http://img.ptcms.csdn.net/article/201507/21/55ae053f548d5_middle.jpg)

###8. <a href="https://github.com/nicklockwood/iRate" target="_blank" title=“iRate”>iRate</a>

想要在App Store中获得更多评论的最佳方式是什么？想要回答这个问题，我缺乏切实数据，但如果必须猜一下的话，我会建议问问用户。也许这样做有点老套——大多开发者现在都有创建定制的应用内置alert。

但是如果你没有时间，或者不想从头实现的话，最好用一下iRata。这个就是iRata——一个小型库——你可以把它放入项目中，把问卷调查什么的都忘记吧，iRate会在恰当的时候为你解决这个问题。

###9. <a href="https://github.com/nihalahmed/GameCenterManager" target="_blank" title=“GameCenterManager”>GameCenterManager</a>

无论你喜不喜欢单例模式，管理一个GameCenter都比我们已知的其他相反模式要好很多。（你的游戏只有一个GameCenter，对吗？）

![9](http://img.ptcms.csdn.net/article/201507/21/55ae05bd3f99e_middle.jpg)

说实话，在iOS上管理GameCenter vanilla并不算难，但是有了这个库会更简单也更快。好上加好不是更好么。


![9-1](http://img.ptcms.csdn.net/article/201507/21/55ae05c9cba37_middle.jpg)

我在我一个游戏中用到了这个，体验很好。

###10. <a href="https://github.com/pkluz/PKRevealController" target="_blank" title=“PKRevealController 2”>PKRevealController 2</a>

这个要注意，真的很棒！是我最喜爱的iOS控件之一。PKRevealController是一个可以滑动的侧边栏菜单（可向左、向右或者同时向两侧），只需手指轻轻一点（或者按一下按钮，但是这样滑动时不够炫酷）。

![2](http://img.ptcms.csdn.net/article/201507/21/55ae067bd3b6b.jpg)

我试过一些可以提供这类控制的其他库，而PKRevealController是最棒的。安装简便，高度定制且对手势识别良好。可以当做一个标准控件用在iOS SDK中。

###11. <a href="https://github.com/slackhq/SlackTextViewController" target="_blank" title=“SlackTextViewController”>SlackTextViewController</a>

你曾经用过Slack iOS应用吗？如果你在较大的软件公司工作，也许会用过。对那些没用过的人呢？—?Slack令人激动。用到Slack的应用也是这样，尤其是用作极佳、定制的文本输入控制时。这时你有了一个现成可用在应用中的代码。

自适应文本区域？试一下。

手势识别、自动填充、多媒体合并？试一下。

快速drop-in解决方案？试一下。

其他还想要什么？

###12. <a href="https://github.com/romaonthego/RETableViewManager" target="_blank" title=“RETableViewManager”>RETableViewManager</a>

RETableViewManager可以帮助你进行动态创建与管理table views。它给我们提供了预定义cells（bool类型、文本、日期等等——请看下面的截图），但是你还可以创建自定义views，并与默认视图一同使用。

![12](http://img.ptcms.csdn.net/article/201507/21/55ae0702088ab_middle.jpg)


左侧截图显得非常古板！在storyboard中没有这个库的时候，你能做的就是这些了，但是有时候代码比可视化编辑器要好。 


###13. <a href="https://github.com/nickoneill/PermissionScope" target="_blank" title=“PermissionScope”>PermissionScope</a>

用这个库可以在询问用户前，就告知用户所需的系统权限，为用户带来更好的体验。接受度更高—>更多活跃用户->更高的留存率->数据更好->下载率更高。墙裂推荐pod。

![13](http://img.ptcms.csdn.net/article/201507/21/55ae0733b6110_middle.jpg)

###14. <a href="https://github.com/TransitApp/SVProgressHUD" target="_blank" title=“SVProgressHUD”>SVProgressHUD</a>

这张图片在正常加载，无需等待太久或者刷新页面。这正是SVProgressHUD在你的应用中的表现。如果你需要定制化的等待提示器，这个就是了（也许是最好的）。

![14](http://img.ptcms.csdn.net/article/201507/21/55ae077d15432.jpg)

###15. <a href="https://github.com/PrideChung/FontAwesomeKit" target="_blank" title=“FontAwesomeKit”>FontAwesomeKit</a>

Font Awesome很棒，有了它你可以很容易地给项目加字体，使用方式繁多。

![15](http://img.ptcms.csdn.net/article/201507/21/55ae07a34b492_middle.jpg)

###16. <a href="https://github.com/SnapKit/SnapKit" target="_blank" title=“SnapKit”>SnapKit</a>

喜欢自动布局吗？当然喜欢！至少在storyboard中创建时会喜欢。 在代码中纯手工创建约束灰常痛苦，但幸运的是我们有了SnapKit，在board中用上它，你可以简单直观地编写约束了。 

![16](http://img.ptcms.csdn.net/article/201507/21/55ae07ebf3c2c_middle.jpg)

###17. <a href="https://github.com/MortimerGoro/MGSwipeTableCell" target="_blank" title=“MGSwipeTableCell”>MGSwipeTableCell</a>

这是另一个常见于很多应用中的UI组件，苹果应该考虑在标准的iOS SDK中加入一些类似的内容。Swipeable表格cell是这个pod的最佳描述，也是最好的。

![17](http://img.ptcms.csdn.net/article/201507/21/55ae0953304a9.jpg)

这只是其中三个动画类型，还有更多变化，请查看readme。

###18. <a href="https://github.com/Quick/Quick" target="_blank" title=“Quick”>Quick</a>

用于Swift中的单元测试（也可用于Objective-C），与Xcode整合在一起。如果你是Objective-C的粉丝，我建议用Specta代替这个，但是对Swift使用者来说，Quick是最佳选择。


![18](http://img.ptcms.csdn.net/article/201507/21/55ae0a6012068_middle.jpg)

![181](http://img.ptcms.csdn.net/article/201507/21/55ae0a767198e_middle.jpg)


###19. <a href="https://github.com/saturngod/IAPHelper" target="_blank" title=“IAPHelper”>IAPHelper</a>

应用内付费给我们提供了很多样本代码，而这个库丢掉了那些代码，将金钱交易相关的大多通用任务做了简单的封装。

###20. <a href="https://github.com/ReactiveCocoa/ReactiveCocoa" target="_blank" title=“ReactiveCocoa”>ReactiveCocoa</a>

好吧，这是个小怪物。

ReactiveCocoa并不像列表中其他库一样，它不是小型的drop-in项目。ReactiveCocoa给我们带来了一个迥异的编程风格与结构，它是基于信号与数据流的。首先你需要忘掉已知的一切，才能理解它的工作方式。很有难度，但是价值不斐。


![20](http://img.ptcms.csdn.net/article/201507/21/55ae0acf1f0dc_middle.jpg)

在这里教ReactiveCocoa有点不合适，但是如果你感兴趣的话，我会提供一些好源：

* <a href="http://www.teehanlax.com/blog/getting-started-with-reactivecocoa/" target="_blank" title=“Getting Started with ReactiveCocoa”>Getting Started with ReactiveCocoa</a>
* <a href="http://nshipster.com/reactivecocoa/" target="_blank" title=“Mattt Thompson：Reactive​Cocoa”>Mattt Thompson：Reactive​Cocoa</a>
* <a href="http://www.raywenderlich.com/62699/reactivecocoa-tutorial-pt1" target="_blank" title=“ReactiveCocoa Tutorial – The Definitive Introduction: Part 1/2”>ReactiveCocoa Tutorial – The Definitive Introduction: Part 1/2</a>

注意：对我们iOS开发社区的朋友来说，这会是一个稍微有些技术含量的活儿。

###21. <a href="https://github.com/SwiftyJSON/SwiftyJSON" target="_blank" title=“SwiftyJSON”>SwiftyJSON</a>

使Swift的JSON解析变得简单。

###22. <a href="https://github.com/MengTo/Spring" target="_blank" title=“Spring”>Spring</a>

使动画在简单性、可链接性与声明性方面有所提高。

![22](http://img.ptcms.csdn.net/article/201507/21/55ae0dfe58b6c_middle.jpg)


###23. <a href="https://github.com/ArtSabintsev/FontBlaster" target="_blank" title=“FontBlaster”>FontBlaster</a>

载入定制字体时更简单。

###24. <a href="https://github.com/JanC/TAPromotee" target="_blank" title=“TAPromotee”>TAPromotee</a>

交叉推广应用是你可以免费实现的最佳市场推广策略之一。使用这个库做起来非常简单，不用都不可能——将TAPromotee加入你的podfile中，免费配置与享受更多下载吧。


![24](http://img.ptcms.csdn.net/article/201507/21/55ae0e81d6464_middle.jpg)


###25. <a href="https://github.com/contentful-labs/Concorde" target="_blank" title=“Concorde”>Concorde</a>

你在应用中载入了一堆jpeg吗？有了Concorde，你可以用更好的方式来解决，这是一个很大的进步。


![25](http://img.ptcms.csdn.net/article/201507/21/55ae11e47390e.jpg)

###26. <a href="https://github.com/kishikawakatsumi/KeychainAccess" target="_blank" title=“KeychainAccess”>KeychainAccess</a>

管理Keychain接入的小助手。

![26](http://img.ptcms.csdn.net/article/201507/21/55ae0f198a4dc_middle.jpg)

###27. <a href="https://github.com/danielgindi/ios-charts" target="_blank" title=“iOS-charts”>iOS-charts</a>

最后一个，但绝不是最不重要的——iOS图表库！非常有用而且美观，这里我无需赘言。向下看，你就知道用它可以做什么了。

![27](http://img.ptcms.csdn.net/article/201507/21/55ae0f6715aec_middle.jpg)

没错，一切都变成了drop-in组件了（也许是“code-in组件）。

![271](http://img.ptcms.csdn.net/article/201507/21/55ae0f7ec6743_middle.jpg)

不幸的是，它还不支持CocoaPods，所以你得手动把它拽到你的Xcode workspace里面去。

超长慎入列表：

1. <a href="https://github.com/dzenbot/DZNEmptyDataSet" target="_blank" title=“DZNEmptyDataSet”>DZNEmptyDataSet</a>（UI，空表格视图解算器）
2. <a href="https://github.com/jivesoftware/PDTSimpleCalendar" target="_blank" title=“PDTSimpleCalendar”>PDTSimpleCalendar</a>（UI，drop-in日历组件）
3. <a href="https://github.com/magicalpanda/MagicalRecord" target="_blank" title=“MagicalRecord”>MagicalRecord</a>（实施活跃记录模式的Core Data助手）
4. <a href="https://github.com/ViccAlexander/Chameleon" target="_blank" title=“Chameleon”>Chameleon</a>（UI，色彩框架）
5. <a href="https://github.com/Alamofire/Alamofire" target="_blank" title=“Alamofire”>Alamofire</a>（Swift 网络）
6. <a href="https://github.com/raulriera/TextFieldEffects" target="_blank" title=“TextFieldEffects”>TextFieldEffects</a> （UI，自定义外观的文本区域）
7. <a href="https://github.com/BradLarson/GPUImage" target="_blank" title=“GPUImage”>GPUImage</a>（快速图片处理）
8. <a href="https://github.com/nicklockwood/iRate" target="_blank" title=“iRate”>iRate</a>（获取用户评价）
9. <a href="https://github.com/nihalahmed/GameCenterManager" target="_blank" title=“GameCenterManager”>GameCenterManager</a>（快速管理游戏中心）
10. <a href="https://github.com/pkluz/PKRevealController" target="_blank" title=“PKRevealController 2”>PKRevealController 2</a>（UI，滑动侧边栏菜单）
11. <a href="https://github.com/slackhq/SlackTextViewController" target="_blank" title=“SlackTextViewController”>SlackTextViewController</a>（UI，高度可定制的自定义文本区域）
12. <a href="https://github.com/romaonthego/RETableViewManager" target="_blank" title=“RETableViewManager”>RETableViewManager</a>（自动以代码创建表格视图）
13. <a href="https://github.com/nickoneill/PermissionScope" target="_blank" title=“PermissionScope”>PermissionScope</a>（UI，预询问用户系统权限的友好体验）
14. <a href="https://github.com/TransitApp/SVProgressHUD" target="_blank" title=“SVProgressHUD”>SVProgressHUD</a> （UI，自定义等待进度条）
15. <a href="https://github.com/PrideChung/FontAwesomeKit" target="_blank" title=“FontAwesomeKit”>FontAwesomeKit</a> （快速添加Awesome字体）
16. <a href="https://github.com/SnapKit/SnapKit" target="_blank" title=“SnapKit”>SnapKit</a>（在代码中快速自动布局）
17. <a href="https://github.com/MortimerGoro/MGSwipeTableCell" target="_blank" title=“MGSwipeTableCell”>MGSwipeTableCell</a> （UI，可滑动的表格视图cell）
18. <a href="https://github.com/Quick/Quick" target="_blank" title=“Quick”>Quick</a>（Swift单元测试框架）
19. <a href="https://github.com/saturngod/IAPHelper" target="_blank" title=“IAPHelper”>IAPHelper</a>（应用内购买助手封装）
20. <a href="https://github.com/ReactiveCocoa/ReactiveCocoa" target="_blank" title=“ReactiveCocoa”>ReactiveCocoa</a>（FRP框架）
21. <a href="https://github.com/SwiftyJSON/SwiftyJSON" target="_blank" title=“SwiftyJSON”>SwiftyJSON</a>（Swift JSON库）
22. <a href="https://github.com/MengTo/Spring" target="_blank" title=“Spring”>Spring</a>（动画框架）
23. <a href="https://github.com/ArtSabintsev/FontBlaster" target="_blank" title=“FontBlaster”>FontBlaster</a>（在应用中快速载入个性化字体）
24. <a href="https://github.com/JanC/TAPromotee" target="_blank" title=“TAPromotee”>TAPromotee</a>（使用drop-in视图交叉推广应用）
25. <a href="https://github.com/contentful-labs/Concorde" target="_blank" title=“Concorde”>Concorde</a>（下载解码渐进式jpeg）
26. <a href="https://github.com/kishikawakatsumi/KeychainAccess" target="_blank" title=“KeychainAccess”>KeychainAccess</a>（简便管理keychain）
27. <a href="https://github.com/danielgindi/ios-charts" target="_blank" title=“iOS-charts”>iOS-charts</a>（美观的图表库）

文章来源：<a href="https://medium.com/app-coder-io/27-ios-open-source-libraries-to-skyrocket-your-development-301b67d3124c" target="_blank" title=“Medium”>Medium</a>
