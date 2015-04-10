---
layout: post
title: "NSMutableURLRequest和NSURLConnection用Post方式上传照片"
date: 2014-12-11 10:00:07 +0800
comments: true
categories: iOS开发
keywords: iOS, NSURLConnection, NSMutableURLRequest, Post, 上传照片,  iOS开发, gcd, cocoapods, 队列, 个人博客, 刚刚在线
description: "NSMutableURLRequest和NSURLConnection用Post方式上传照片"


---

需求：上传一张照片和内容，内容包括：用户编辑的内容、经纬度、地点等等，就是类似发一个说说。这就是一个很平常的功能，实现的方式想必各位能想到很多。

接下来我想用系统的NSURLConnection来实现这个功能：

首先我们要定义一个全局变量:

	/**
	 *  请求返回数据
	 */
	@property (nonatomic, strong) NSMutableData *mResponseData;


实现代码：

	//字典里面装的是你要上传的内容
	NSDictionary *parameters = @{@"content": @"这是刚刚在线的官方网站www.superqq.com"};

	//上传的接口
    NSString* urlstring = @"http://superqq.com/";
    //分界线的标识符
    NSString *TWITTERFON_FORM_BOUNDARY = @"AaB03x";
    //根据url初始化request
    NSMutableURLRequest * request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:urlstring]
                                                            cachePolicy:NSURLRequestReloadIgnoringLocalCacheData
                                                        timeoutInterval:10];
    //分界线 --AaB03x
    NSString *MPboundary=[[NSString alloc]initWithFormat:@"--%@",TWITTERFON_FORM_BOUNDARY];
    //结束符 AaB03x--
    NSString *endMPboundary=[[NSString alloc]initWithFormat:@"%@--",MPboundary];
    //    //要上传的图片
    //    UIImage *image=[params objectForKey:@"pic"];
    //得到图片的data
    NSData *data = UIImagePNGRepresentation(self.image);
    //http body的字符串
    NSMutableString *body=[[NSMutableString alloc]init];
    //参数的集合的所有key的集合
    NSArray *keys= [parameters allKeys];
    
    //遍历keys
    for(int i=0;i<[keys count];i++)
    {
        //得到当前key
        NSString *key=[keys objectAtIndex:i];
        //如果key不是pic，说明value是字符类型，比如name：Boris
        if(![key isEqualToString:@"pic"])
        {
            //添加分界线，换行
            [body appendFormat:@"%@\r\n",MPboundary];
            //添加字段名称，换2行
            [body appendFormat:@"Content-Disposition: form-data; name=\"%@\"\r\n\r\n",key];
            //添加字段的值
            [body appendFormat:@"%@\r\n",[parameters objectForKey:key]];
        }
    }
    
    ////添加分界线，换行
    [body appendFormat:@"%@\r\n",MPboundary];
    //声明pic字段，文件名为boris.png
    [body appendFormat:@"Content-Disposition: form-data; name=\"pic\"; filename=\"boris.png\"\r\n"];
    //声明上传文件的格式
    [body appendFormat:@"Content-Type: image/png\r\n\r\n"];
    
    //声明结束符：--AaB03x--
    NSString *end=[[NSString alloc]initWithFormat:@"\r\n%@",endMPboundary];
    //声明myRequestData，用来放入http body
    NSMutableData *myRequestData=[NSMutableData data];
    //将body字符串转化为UTF8格式的二进制
    [myRequestData appendData:[body dataUsingEncoding:NSUTF8StringEncoding]];
    //将image的data加入
    [myRequestData appendData:data];
    //加入结束符--AaB03x--
    [myRequestData appendData:[end dataUsingEncoding:NSUTF8StringEncoding]];
    
    //设置HTTPHeader中Content-Type的值
    NSString *content=[[NSString alloc]initWithFormat:@"multipart/form-data; boundary=%@",TWITTERFON_FORM_BOUNDARY];
    //设置HTTPHeader
    [request setValue:content forHTTPHeaderField:@"Content-Type"];
    //设置Content-Length
    [request setValue:[NSString stringWithFormat:@"%d", (int)[myRequestData length]] forHTTPHeaderField:@"Content-Length"];
    //设置http body
    [request setHTTPBody:myRequestData];
    //http method
    [request setHTTPMethod:@"POST"];
    
    //建立连接，设置代理
    NSURLConnection *conn = [[NSURLConnection alloc] initWithRequest:request delegate:self];
    
    //设置接受response的data
    if (conn) {
        _mResponseData = [[NSMutableData alloc] init];
    }


NSURLConnectionDelegate的代理方法：

	
	#pragma mark - NSURLConnectionDelegate
	
	- (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response
	{
	   [_mResponseData setLength:0];
	}
	
	- (void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data
	{
	   [_mResponseData appendData:data];
	}
	
	- (void)connectionDidFinishLoading:(NSURLConnection *)connection
	{
	   
	   NSDictionary *dic =  [NSJSONSerialization JSONObjectWithData:_mResponseData options:kNilOptions error:nil];
	   NSLog(@"%@", dic);
	   
	}
	
	- (void)connection:(NSURLConnection *)connection didFailWithError:(NSError *)error
	{
	   NSLog(@"Error: %@", error);
	}


这是我的实现方式，再次抛砖引玉，如果你要好的方法，欢迎分享！

iOS开发微信公众号：

	iOSDevTip
	
我也会将大家分享的方法发到iOS开发微信公众账号：iOSDevTip 里面，分享给更多iOS开发者。

