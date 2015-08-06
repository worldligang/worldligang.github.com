---
layout: post
title: "UITextView实现placeholder的猥琐做法"
date: 2015-08-06 21:37:38 +0800
comments: true
categories: iOS开发
keywords: UITextField, placeholder, text, iOS开发, 刚刚在线
description: 我们都知道iOS开发中的UITextField有个placeholder属性，placeholder可以很方便引导用户输入。但是UITextView却没有placeholder属性。

---

我经常写一些[iOS开发](http://www.superqq.com/)的小技巧，为了在自己需要的时候能用得上，也希望能对正在看文章的你一些帮助，亦或是引发一些思考。

如果你觉得文章对你有帮助，请多多分享。你身边一定有很多做iOS开发的朋友，希望你能把我的iOS开发：**iOSDevTip**推荐给你的朋友。不甚感激！

我们都知道iOS开发中的`UITextField`有个`placeholder`属性，`placeholder`可以很方便引导用户输入。但是`UITextView`却没有`placeholder`属性。

##猥琐法分析

如何让`UITextView`也有`placeholder`功能呢？今天给各位分享一个比较猥琐的做法。思路大概是这样的：

1. 把`UITextView`的`text`当`placeholder`使用。
2. 在开始编辑的代理方法里清除`placeholder`。
3. 在结束编辑的代理方法里在设置`placeholder`。
	
	
	
##实现方法

###创建UITextView	
	
	UITextView *textViewPlaceholder = [[UITextView alloc] initWithFrame:CGRectMake(20, 70, SCREEN.width - 40, 100)];
    textViewPlaceholder.backgroundColor = [UIColor whiteColor];
    textViewPlaceholder.text = @"关注微信公众号iOS开发：iOSDevTip";
    textViewPlaceholder.textColor = [UIColor grayColor];
    textViewPlaceholder.delegate = self;
    [self.view addSubview:textViewPlaceholder];
 
初始化`UITextView`，给`UITextView`的`text`赋值，并且给`UITextView`的`textColor`属性设置成灰色，让其看起来更像`placeholder`。

别忘了设置`UITextView`的代理，因为后面我们要用到`UITextView`的两个代理方法。

###开始编辑的代理方法    
    
    - (void)textViewDidBeginEditing:(UITextView *)textView {
    
	    if ([textView.text isEqualToString:@"关注微信公众号iOS开发：iOSDevTip"]) {
	        textView.text = @"";
	        textView.textColor = [UIColor blackColor];
	    }
	}

在开始编辑的代理方法里面，判断如果是`UITextView`的`text`的值是`placeholder`，那么，就清空`text`，并且把`textColor`设置成真正的内容颜色，假设是黑色。

###结束编辑的代理方法    

	
	- (void)textViewDidEndEditing:(UITextView *)textView {
	    if (textView.text.length<1) {
	        textView.text = @"关注微信公众号iOS开发：iOSDevTip";
	        textView.textColor = [UIColor grayColor];
	    }
	}
	
在结束编辑的代理方法里，判断如果`UITextView`的`text`值为空，那么，就要把需要设置的`placeholder`赋值给`UITextView`的`text`，并且将`textColor`属性设置成灰色。

##添加轻击手势
    
    UITapGestureRecognizer *tapGesture = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(tapGesture:)];
    tapGesture.numberOfTapsRequired = 1; //点击次数
    tapGesture.numberOfTouchesRequired = 1; //点击手指数
    [self.view addGestureRecognizer:tapGesture];
    
    //轻击手势触发方法
	-(void)tapGesture:(UITapGestureRecognizer *)sender
	{
	    [self.view endEditing:YES];
	}

至此，就很猥琐的实现了`placeholder`功能。为了方便测试，我加了一个手势。作用是用键盘消失，这样可以测试结束编辑的时候`placeholder`会不会显示。demo地址：Demo地址:[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)

添加手势的方法很简单，关于iOS开发中的手势汇总，你可以看这篇文章[iOS开发之手势识别汇总](http://www.superqq.com/blog/2015/01/14/ioskai-fa-zhi-shou-shi-shi-bie-hui-zong/)。

我看了网上一些实现`UITextView`的`placeholder`功能的更猥琐的做法，在`UITextView`上面盖一个`UILabel`，然后在`UITextView`的代理方法里控制`UILabel`的值。还有用两个`UITextView`实现的。

在这里，我只是抛砖引玉，如果你有更好的实现方法欢迎投稿给我：**worldligang@163.com**，下一篇文章，我将会封装一个带有`placeholder`属性的`UITextView`，敬请期待！
