---
layout: post
title: "CocoaPods详解之----使用篇"
date: 2014-11-15 13:40:53 +0800
comments: true
categories: iOS开发
keywords: CocoaPods, iOS,pod install,pod update, pod search, pod setup, iOSDevTip iOS开发, gcd, cocoapods, 队列, 个人博客, 刚刚在线
description: "CocoaPods详解之----使用篇" 

---
</br>

<div class="entry-content"><h3>一、什么是CocoaPods</h3>

<div class="entry-content"><h5>1、为什么需要CocoaPods</h5>

在进行iOS开发的时候，总免不了使用第三方的开源库，比如SBJson、AFNetworking、Reachability等等。使用这些库的时候通常需要：

下载开源库的源代码并引入工程

向工程中添加开源库使用到的framework

解决开源库和开源库以及开源库和工程之间的依赖关系、检查重复添加的framework等问题
    
如果开源库有更新的时候，还需要将工程中使用的开源库删除，重新执行前面的三个步骤，顿时头都大了。。。

自从有了<a href="http://www.superqq.com/">CocoaPods</a>以后，这些繁杂的工作就不再需要我们亲力亲为了，只需要我们做好少量的配置工作，CocoaPods会为我们做好一切！

<div class="entry-content"><h5>2、什么是CocoaPods</h5>

CocoaPods是一个用来帮助我们管理第三方依赖库的工具。它可以解决库与库之间的依赖关系，下载库的源代码，同时通过创建一个Xcode的workspace来将这些第三方库和我们的工程连接起来，供我们开发使用。

使用CocoaPods的目的是让我们能自动化的、集中的、直观的管理第三方开源库。


<div class="entry-content"><h3>二、安装CocoaPods</h3>

<div class="entry-content"><h5>1、安装CocoaPods</h5>

CocoaPods是用Ruby实现的，要想使用它首先需要有Ruby的环境。幸运的是OS X系统默认的已经可以运行Ruby了，因此我们只需要执行以下命令：
[objc] view plaincopy在CODE上查看代码片派生到我的代码片

    $ sudo gem install cocoapods  

CocoaPods是以Ruby gem包的形式被安装的。在安装执行的过程中，可能会问我们是不是更新rake，输入y即可。这是因为rake gem包会在安装的过程中检查更细，如果有可用的新版本就会出现刚才的选项。

在安装进程结束的时候，执行命令：
[objc] view plaincopy在CODE上查看代码片派生到我的代码片

    $ pod setup  

如果没有报错，就说明一切安装就成功了！

<div class="entry-content"><h5>2、安装过程中可能遇到的问题</h5>

①执行完install命令半天没反应

这有可能是因为Ruby的默认源使用的是cocoapods.org，国内访问这个网址有时候会有问题，网上的一种解决方案是将远替换成淘宝的，替换方式如下：
[objc] view plaincopy在CODE上查看代码片派生到我的代码片

    $ gem sources --remove https://rubygems.org/  
    //等有反应之后再敲入以下命令  
    $ gem sources -a http://ruby.taobao.org/  

要想验证是否替换成功了，可以执行：
[objc] view plaincopy在CODE上查看代码片派生到我的代码片

    $ gem sources -l  

正常的输出是：
[objc] view plaincopy在CODE上查看代码片派生到我的代码片

    *** CURRENT SOURCES ***  
      
    http://ruby.taobao.org/  

②gem版本过老

gem是管理Ruby库和程序的标准包，如果它的版本过低也可能导致安装失败，解决方案自然是升级gem，执行下述命令即可：
[objc] view plaincopy在CODE上查看代码片派生到我的代码片

    $ sudo gem update --system  

③安装完成后，执行pod setup命令时报错：
[objc] view plaincopy在CODE上查看代码片派生到我的代码片

    /Users/wangzz/.rvm/rubies/ruby-1.9.3-p448/lib/ruby/site_ruby/1.9.1/rubygems/dependency.rb:298:in `to_specs': Could not find 'cocoapods' (>= 0) among 6 total gem(s) (Gem::LoadError)  
        from /Users/wangzz/.rvm/rubies/ruby-1.9.3-p448/lib/ruby/site_ruby/1.9.1/rubygems/dependency.rb:309:in `to_spec'  
        from /Users/wangzz/.rvm/rubies/ruby-1.9.3-p448/lib/ruby/site_ruby/1.9.1/rubygems/core_ext/kernel_gem.rb:53:in `gem'  
        from /Users/wangzz/.rvm/rubies/ruby-1.9.3-p448/bin/pod:22:in `<main>'  

这就是路径设置的问题，可以通过执行：
[objc] view plaincopy在CODE上查看代码片派生到我的代码片

    $ rvm use ruby-1.9.3-p448  

解决该问题。

<div class="entry-content"><h5>3、升级CocoaPods</h5>


升级很简单，再次执行安装命令即可：
[objc] view plaincopy在CODE上查看代码片派生到我的代码片

    $ sudo gem install cocoapods  

需要注意的是，如果安装的时候使用了sudo，升级的时候一样需要使用该关键字，不然升级完了以后又会出现路径不匹配问题。


<div class="entry-content"><h3>三、使用CocoaPods</h3>

如果之前做的一切顺利，接下来就可以体验体验<a href="http://www.superqq.com/">CocoaPods</a>的神奇之处了，需要经过以下几步：

为了演示这个过程，我创建了一个名为CocoaPodsTest的工程。
<div class="entry-content"><h5>1、创建Podfile</h5>


CocoaPods的一切都是从一个名为Podfile的文件开始的，我们需要先创建这个文件。个人习惯使用命令行，我会这样做：
[objc] view plaincopy在CODE上查看代码片派生到我的代码片

    $ cd /Users/wangzz/Desktop/CocoaPodsTest  
    $ touch Podfile  

首先进入到工程的根目录下，创建空白的Podfile文件，创建完毕的目录结构如下图：

<img src="http://img.blog.csdn.net/20140126162345078?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3p6dmljdG9yeQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast">


（PS：Podfile文件也可以不放在工程的根目录下，只是会稍微麻烦点，在下一篇文章中会有介绍，敬请关注。）
<div class="entry-content"><h5>2、编辑Podfile</h5>


根据需要，我们可以在Podfile文件中写入需要用到的第三方库，以SBJson、AFNetworking、Reachability三个库为例，我的Podfile内容如下：
[objc] view plaincopy在CODE上查看代码片派生到我的代码片

    platform :ios  
    pod 'Reachability',  '~> 3.0.0'  
    pod 'SBJson', '~> 4.0.0'  
      
    platform :ios, '7.0'  
    pod 'AFNetworking', '~> 2.0'  

<div class="entry-content"><h5>3、执行导入命令</h5>


准备工作都完成后，开始导入第三方库：
[objc] view plaincopy在CODE上查看代码片派生到我的代码片

    $ cd /Users/wangzz/Desktop/CocoaPodsTest  
    $ pod install  

首先进入工程根目录，然后执行pod install命令，CocoaPods就开始为我们做下载源码、配置依赖关系、引入需要的framework等一些列工作，命令的执行结果打印出来如下：
[objc] view plaincopy在CODE上查看代码片派生到我的代码片

    Analyzing dependencies  
    Downloading dependencies  
    Installing AFNetworking (2.1.0)  
    Installing JSONKit (1.5pre)  
    Installing Reachability (3.0.0)  
    Generating Pods project  
    Integrating client project  
      
    [!] From now on use `CocoaPodsTest.xcworkspace`.  

这就说明pod install命令执行成功了。再来看看工程根目录发生的变化，如下图：

<img src="http://img.blog.csdn.net/20140126163651109?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3p6dmljdG9yeQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast">


可以看到，工程的根目录下多了三个东西：CocoaPodsTest.xcworkspace、Podfile.lock文件和Pods目录。


<div class="entry-content"><h5>（PS：篇幅有限，Podfile.lock文件会放到系列文章的下一篇介绍，敬请关注。）</h5>


再看看刚才执行完pod install命令打印出来的内容的最后一行：
[objc] view plaincopy在CODE上查看代码片派生到我的代码片

    [!] From now on use `CocoaPodsTest.xcworkspace`.  

提示我们从现在起，我们需要使用CocoaPodsTest.xcworkspace文件来开发。

对于工程发生的变化，有几点需要说明：

    第三方库会被编译成静态库供我们正真的工程使用

CocoaPods会将所有的第三方库以target的方式组成一个名为Pods的工程，该工程就放在刚才新生成的Pods目录下。整个第三方库工程会生成一个名称为libPods.a的静态库提供给我们自己的CocoaPodsTest工程使用。

    我们的工程和第三方库所在的工程会由一个新生成的workspace管理

为了方便我们直观的管理工程和第三方库，CocoaPodsTest工程和Pods工程会被以workspace的形式组织和管理，也就是我们刚才看到的CocoaPodsTest.xcworkspace文件。

原来的工程设置已经被更改了，这时候我们直接打开原来的工程文件去编译就会报错，只能使用新生成的workspace来进行项目管理。

打开CocoaPodsTest.xcworkspace，界面如下：

<img src="http://img.blog.csdn.net/20140126171045546?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3p6dmljdG9yeQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast">


工程的目录结构还是非常明显的。

在项目中引用刚才添加的第三方库的头文件，执行编译操作，果断成功！

至此，CocoaPods使用篇告一段落，接下来打算再用一个篇幅介绍CocoaPods稍微深层次的东西，敬请关注。


<div class="entry-content"><h3>四、参考资料</h3>

<a href="http://code4app.com/article/cocoapods-install-usage"target="_blank"title="刚刚在线">1、http://code4app.com/article/cocoapods-install-usage</a>  


<a href="http://cocoapods.org/"target="_blank"title="刚刚在线">2、http://cocoapods.org/</a>  

</br>

我的微信公众号 iOS开发 ：

	iOSDevTip
	
</br>

文章来自： <a href="http://blog.csdn.net/wzzvictory/article/details/18737437"target="_blank"title="CocoaPods详解之----使用篇">CocoaPods详解之----使用篇</a> 


