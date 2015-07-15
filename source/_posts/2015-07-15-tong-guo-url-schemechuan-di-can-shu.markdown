---
layout: post
title: "通过URL Scheme传递参数"
date: 2015-07-15 22:50:21 +0800
comments: true
categories: iOS开发
keywords: URL Scheme, URL Identifier, Xcode6.4, Scheme, 刚刚在线
description: 通过URL Scheme启动APP很简单就可以做到，但有时候我们想在启动APP的时候传递一些参数，这个时候我们就可以通过URL Scheme自定义URL来传递参数了。

---

有这么一个段子特别火：实体店终于翻身了，他们用铁一样的事实告诉淘宝和京东，互联网+再牛逼也干不过实体店。你再便宜，你有试衣间吗？百年修得同船渡，千年修得优衣库。众里寻他千百度，三里屯优衣库。最美的不是下雨天，而是和你在试衣间。

优衣库火了，你的朋友圈有没有被优衣库刷屏呀？据说还有视频传出来，各种版本的。有人竟然从我要视频，我是给还是不给呢？不知道优衣库的童鞋，赶紧百度一下吧！

##这会是优衣库的炒作吗？

优衣库事件让优衣库狠狠的火了一把。不禁发问：这会是优衣库的炒作吗？不得而知。事件一经各大媒体报道出来之后，优衣库立刻发出声明。此事和我们没有半毛钱关系。

如果这真是一次炒作。那么优衣库的营销未免太低俗了，这将会大大降低优衣库在消费者心里的地位。以后来优衣库的人可能不是买衣服而是约炮。

如果这不是一次炒作。那么，这段视频上传的目的是什么呢？是男女主角想借此出名吗？可能性不是特别高，借此出名有什么好的呢！真正的目的也许只有当事人心里最清楚。

我问了北京的朋友，是否知道优衣库事件。朋友说，何止知道，天天从那里路过。现在有很多人在那里合影呢，三里屯优衣库俨然成了一个景点。

**热点总是一时的，借此炒作的人或许已达成目的。**作为一名iOS开发程序员，我们只能继续写代码，改变世界。

上一篇文章讲到：[Xcode6.4注册URL Scheme步骤详解](http://www.superqq.com/blog/2015/07/14/xcode6-dot-4zhu-ce-url-schemebu-zou-xiang-jie/)，这一篇继续来看看URL Scheme传递参数。

##通过注册的URL Scheme向目标APP传递参数

通过URL Scheme启动APP很简单就可以做到，但有时候我们想在启动APP的时候传递一些参数，这个时候我们就可以通过URL Scheme自定义URL来传递参数了。

昨天我们在AppDelegate调用了UIApplicationDelegate的代理方法：

	- (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL*)url
	{
	    // 接受传过来的参数
	    NSString *text = [[url host] stringByReplacingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
	    UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"打开啦"
	                                           message:text
	                                          delegate:nil
	                                 cancelButtonTitle:@"OK"
	                                 otherButtonTitles:nil];
	    [alertView show];
	    return YES;
	}

我们来看看苹果给这个方法的注释：

	- (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url;  // Will be deprecated at some point, please replace with application:openURL:sourceApplication:annotation:

这个方法在未来将被废弃，可以用application:openURL:sourceApplication:annotation:来代替。

##URL传参格式

昨天我们在[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)注册的URL Scheme还记得是什么吗？你应该还有印象的就是iOS开发的ID：**iOSDevTip**。

假设我们想要传递两个参数分别是名字name和手机号phone，格式如下：

	iOSDevTip://?name=ligang&phone=13888888888
	
有没有似曾相识的感觉。我们用get方式请求一个接口是不是就是这样的。

##被启动的APP处理传过来的参数

	- (BOOL)application:(UIApplication *)application
            openURL:(NSURL *)url
	  sourceApplication:(NSString *)sourceApplication
	         annotation:(id)annotation
	{
	    NSLog(@"sourceApplication: %@", sourceApplication);
	    NSLog(@"URL scheme:%@", [url scheme]);
	    NSLog(@"URL query: %@", [url query]);
	    
	    // 接受传过来的参数
	    UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"打开啦"
	                                                        message:[url query]
	                                                       delegate:nil
	                                              cancelButtonTitle:@"OK"
	                                              otherButtonTitles:nil];
	    [alertView show];
	    
	    return YES;
	}

当APP被启动是，会调用代理方法application:openURL:sourceApplication:annotation:。参数URL就是启动APP的URL，参数sourceApplication就是来源APP的Bundle ID。

我们依然通过Safari来测试，在Safari的地址栏中输入：iOSDevTip://?name=ligang&phone=13888888888

![scheme1](http://7xjrlb.com1.z0.glb.clouddn.com/scheme1.png)

即可打开APP,看看参数是否传递过来：

![scheme2](http://7xjrlb.com1.z0.glb.clouddn.com/scheme2.png)

最后我们看一下打印：

	2015-07-15 22:38:25.655 iOSStrongDemo[9983:2894855] sourceApplication: com.apple.mobilesafari
	2015-07-15 22:38:28.664 iOSStrongDemo[9983:2894855] URL scheme:iosdevtip
	2015-07-15 22:38:28.665 iOSStrongDemo[9983:2894855] URL query: name=ligang&phone=13888888888


sourceApplication打印出来是com.apple.mobilesafari，从这里可以看出来，是从Safari启动我们的APP的。

我们虽然自定义了URL Scheme，但是我们不能阻止别人通过自定义的URL Scheme来打开我们的应用。怎么解决呢？

我们可以指定相应的sourceApplication，也就是相应的Bundle ID，通过Bundle ID来决定是否可以打开我们的APP：

	- (BOOL)application:(UIApplication *)application
            openURL:(NSURL *)url
	  sourceApplication:(NSString *)sourceApplication
	         annotation:(id)annotation
	{
	    NSLog(@"sourceApplication: %@", sourceApplication);
	    NSLog(@"URL scheme:%@", [url scheme]);
	    NSLog(@"URL query: %@", [url query]);
	    
	    if ([sourceApplication isEqualToString:@"com.3Sixty.CallCustomURL"]){
	        // 接受传过来的参数
	        UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"打开啦"
	                                                            message:[url query]
	                                                           delegate:nil
	                                                  cancelButtonTitle:@"OK"
	                                                  otherButtonTitles:nil];
	        [alertView show];
	        
	        return YES;
	    }else{
	        return NO;
	    }
	    
	}
	
这样我们就可以通过Bundle ID来决定是否允许打开我们的APP，demo下载地址：[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)
