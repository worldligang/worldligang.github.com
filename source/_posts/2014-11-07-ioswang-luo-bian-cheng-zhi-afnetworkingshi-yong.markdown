---
layout: post
title: "iOS网络编程之AFNetworking使用"
date: 2014-11-07 11:07:32 +0800
comments: true
categories: iOS开发
keywords: iOS, iOS网络编程, AFNetworking, iOSDevTip iOS开发, gcd, cocoapods, 队列, 个人博客, 刚刚在线
description: "iOS网络编程之AFNetworking使用"  

---

<a href="http://www.superqq.com/blog/2014/11/07/ioswang-luo-bian-cheng-zhi-afnetworkingshi-yong/">iOS网络编程之AFNetworking使用</a>

现在大家都喜欢用AFNetworking，因为有人维护。看来一下官方文档，把使用的代码简单罗列一下：

首先通过<a href="http://www.superqq.com/blog/2014/10/16/cocoapodsan-zhuang-he-shi-yong-jiao-cheng/">CocoaPods</a>(没用过的看这篇文章)把AFNetworking拉下来

Podfile
	
	platform :ios, '7.0'
	pod "AFNetworking", "~> 2.0"
	
方法一：GET 请求

	AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
	[manager GET:@"http://example.com/resources.json" parameters:nil success:^(AFHTTPRequestOperation *operation, id responseObject) {
	    NSLog(@"JSON: %@", responseObject);
	} failure:^(AFHTTPRequestOperation *operation, NSError *error) {
	    NSLog(@"Error: %@", error);
	}];
	
方法二：POST 请求

	AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
	NSDictionary *parameters = @{@"foo": @"bar"};
	[manager POST:@"http://example.com/resources.json" parameters:parameters success:^(AFHTTPRequestOperation *operation, id responseObject) {
	    NSLog(@"JSON: %@", responseObject);
	} failure:^(AFHTTPRequestOperation *operation, NSError *error) {
	    NSLog(@"Error: %@", error);
	}];
	
方法三：POST Multi-Part Request

	AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
	NSDictionary *parameters = @{@"foo": @"bar"};
	NSURL *filePath = [NSURL fileURLWithPath:@"file://path/to/image.png"];
	[manager POST:@"http://example.com/resources.json" parameters:parameters constructingBodyWithBlock:^(id<AFMultipartFormData> formData) {
	    [formData appendPartWithFileURL:filePath name:@"image" error:nil];
	} success:^(AFHTTPRequestOperation *operation, id responseObject) {
	    NSLog(@"Success: %@", responseObject);
	} failure:^(AFHTTPRequestOperation *operation, NSError *error) {
	    NSLog(@"Error: %@", error);
	}];
	
方法四：创建一个下载文件任务

	NSURLSessionConfiguration *configuration = [NSURLSessionConfiguration defaultSessionConfiguration];
	AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:configuration];
	
	NSURL *URL = [NSURL URLWithString:@"http://example.com/download.zip"];
	NSURLRequest *request = [NSURLRequest requestWithURL:URL];
	
	NSURLSessionDownloadTask *downloadTask = [manager downloadTaskWithRequest:request progress:nil destination:^NSURL *(NSURL *targetPath, NSURLResponse *response) {
	    NSURL *documentsDirectoryURL = [[NSFileManager defaultManager] URLForDirectory:NSDocumentDirectory inDomain:NSUserDomainMask appropriateForURL:nil create:NO error:nil];
	    return [documentsDirectoryURL URLByAppendingPathComponent:[response suggestedFilename]];
	} completionHandler:^(NSURLResponse *response, NSURL *filePath, NSError *error) {
	    NSLog(@"File downloaded to: %@", filePath);
	}];
	[downloadTask resume];
	
方法五：创建一个上传文件任务

	NSURLSessionConfiguration *configuration = [NSURLSessionConfiguration defaultSessionConfiguration];
	AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:configuration];
	
	NSURL *URL = [NSURL URLWithString:@"http://example.com/upload"];
	NSURLRequest *request = [NSURLRequest requestWithURL:URL];
	
	NSURL *filePath = [NSURL fileURLWithPath:@"file://path/to/image.png"];
	NSURLSessionUploadTask *uploadTask = [manager uploadTaskWithRequest:request fromFile:filePath progress:nil completionHandler:^(NSURLResponse *response, id responseObject, NSError *error) {
	    if (error) {
	        NSLog(@"Error: %@", error);
	    } else {
	        NSLog(@"Success: %@ %@", response, responseObject);
	    }
	}];
	[uploadTask resume];

方法六：创建一个上传文件任务并显示进度

	NSMutableURLRequest *request = [[AFHTTPRequestSerializer serializer] multipartFormRequestWithMethod:@"POST" URLString:@"http://example.com/upload" parameters:nil constructingBodyWithBlock:^(id<AFMultipartFormData> formData) {
	        [formData appendPartWithFileURL:[NSURL fileURLWithPath:@"file://path/to/image.jpg"] name:@"file" fileName:@"filename.jpg" mimeType:@"image/jpeg" error:nil];
	    } error:nil];
	
	AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]];
	NSProgress *progress = nil;
	
	NSURLSessionUploadTask *uploadTask = [manager uploadTaskWithStreamedRequest:request progress:&progress completionHandler:^(NSURLResponse *response, id responseObject, NSError *error) {
	    if (error) {
	        NSLog(@"Error: %@", error);
	    } else {
	        NSLog(@"%@ %@", response, responseObject);
	    }
	}];
	[uploadTask resume];

方法七：创建一个上传数据data任务

	NSURLSessionConfiguration *configuration = [NSURLSessionConfiguration defaultSessionConfiguration];
	AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:configuration];
	
	NSURL *URL = [NSURL URLWithString:@"http://example.com/upload"];
	NSURLRequest *request = [NSURLRequest requestWithURL:URL];
	
	NSURLSessionDataTask *dataTask = [manager dataTaskWithRequest:request completionHandler:^(NSURLResponse *response, id responseObject, NSError *error) {
	    if (error) {
	        NSLog(@"Error: %@", error);
	    } else {
	        NSLog(@"%@ %@", response, responseObject);
	    }
	}];
	[dataTask resume];
	
方法八：获取网络状态

	[[AFNetworkReachabilityManager sharedManager] setReachabilityStatusChangeBlock:^(AFNetworkReachabilityStatus status) {
	    NSLog(@"Reachability: %@", AFStringFromNetworkReachabilityStatus(status));
	}]; 
	

方法九： HTTP Manager Reachability

	NSURL *baseURL = [NSURL URLWithString:@"http://example.com/"];
	AFHTTPRequestOperationManager *manager = [[AFHTTPRequestOperationManager alloc] initWithBaseURL:baseURL];
	
	NSOperationQueue *operationQueue = manager.operationQueue;
	[manager.reachabilityManager setReachabilityStatusChangeBlock:^(AFNetworkReachabilityStatus status) {
	    switch (status) {
	        case AFNetworkReachabilityStatusReachableViaWWAN:
	        case AFNetworkReachabilityStatusReachableViaWiFi:
	            [operationQueue setSuspended:NO];
	            break;
	        case AFNetworkReachabilityStatusNotReachable:
	        default:
	            [operationQueue setSuspended:YES];
	            break;
	    }
	}];
	
	[manager.reachabilityManager startMonitoring];


方法十：AFHTTPRequestOperation的GET请求

	NSURL *URL = [NSURL URLWithString:@"http://example.com/resources/123.json"];
	NSURLRequest *request = [NSURLRequest requestWithURL:URL];
	AFHTTPRequestOperation *op = [[AFHTTPRequestOperation alloc] initWithRequest:request];
	op.responseSerializer = [AFJSONResponseSerializer serializer];
	[op setCompletionBlockWithSuccess:^(AFHTTPRequestOperation *operation, id responseObject) {
	    NSLog(@"JSON: %@", responseObject);
	} failure:^(AFHTTPRequestOperation *operation, NSError *error) {
	    NSLog(@"Error: %@", error);
	}];
	[[NSOperationQueue mainQueue] addOperation:op];  

方法十一：Batch of Operations

	NSMutableArray *mutableOperations = [NSMutableArray array];
	for (NSURL *fileURL in filesToUpload) {
	    NSURLRequest *request = [[AFHTTPRequestSerializer serializer] multipartFormRequestWithMethod:@"POST" URLString:@"http://example.com/upload" parameters:nil constructingBodyWithBlock:^(id<AFMultipartFormData> formData) {
	        [formData appendPartWithFileURL:fileURL name:@"images[]" error:nil];
	    }];
	
	    AFHTTPRequestOperation *operation = [[AFHTTPRequestOperation alloc] initWithRequest:request];
	
	    [mutableOperations addObject:operation];
	}
	
	NSArray *operations = [AFURLConnectionOperation batchOfRequestOperations:@[...] progressBlock:^(NSUInteger numberOfFinishedOperations, NSUInteger totalNumberOfOperations) {
	    NSLog(@"%lu of %lu complete", numberOfFinishedOperations, totalNumberOfOperations);
	} completionBlock:^(NSArray *operations) {
	    NSLog(@"All operations in batch complete");
	}];
	[[NSOperationQueue mainQueue] addOperations:operations waitUntilFinished:NO];


方法十二：获取请求的一些信息(我也没有用过，不太常用)  



Request Serialization

Request serializers create requests from URL strings, encoding parameters as either a query string or HTTP body.

	NSString *URLString = @"http://example.com";
	NSDictionary *parameters = @{@"foo": @"bar", @"baz": @[@1, @2, @3]};

Query String Parameter Encoding

	[[AFHTTPRequestSerializer serializer] requestWithMethod:@"GET" URLString:URLString parameters:parameters error:nil];

	GET http://example.com?foo=bar&baz[]=1&baz[]=2&baz[]=3

URL Form Parameter Encoding

	[[AFHTTPRequestSerializer serializer] requestWithMethod:@"POST" URLString:URLString parameters:parameters];

	POST http://example.com/
	Content-Type: application/x-www-form-urlencoded

	foo=bar&baz[]=1&baz[]=2&baz[]=3

JSON Parameter Encoding

	[[AFJSONRequestSerializer serializer] requestWithMethod:@"POST" URLString:URLString parameters:parameters];

	POST http://example.com/
	Content-Type: application/json

	{"foo": "bar", "baz": [1,2,3]}



我的微信公众号 iOS开发 ： 
	
	iOSDevTip






