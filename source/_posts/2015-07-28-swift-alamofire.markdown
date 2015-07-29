---
layout: post
title: "著名的AFNetworking网络基础库Swift版Alamofire"
date: 2015-07-28 20:42:52 +0800
comments: true
categories: swift开发
keywords: Alamofire, swift,第三方, iOS开发, 刚刚在线
description: Alamofire是Swift语言的HTTP网络开发工具包,相当于Swift实现AFNetworking版本。

---


<a href="https://github.com/Alamofire/Alamofire" target="_blank" title=“源代码”>源代码</a>

![swift-gif](http://swiftmi.qiniudn.com/codeimage/201409/13_1410874399080_89.gif)

##Swift Alamofire 简介


![Alamofire: Elegant Networking in Swift](https://raw.githubusercontent.com/Alamofire/Alamofire/assets/alamofire.png)

Alamofire是 Swift 语言的 HTTP 网络开发工具包,相当于Swift实现[AFNetworking](http://www.superqq.com/blog/2014/11/07/ioswang-luo-bian-cheng-zhi-afnetworkingshi-yong/)版本。

当然,AFNetworking非常稳定,在Mac OSX与iOS中也能像其他Objective-C代码一样用Swift编写。不过Alamofire更适合Swift语言风格习惯(Alamofire与AFNetworking可以共存一个项目中,互不影响).

    Alamofire 取名来源于 [Alamo Fire flower](https://aggie-horticulture.tamu.edu/wildseed/alamofire.html)

##Alamofire安装使用方法

使用CocoaPods安装，在podfile

	source 'https://github.com/CocoaPods/Specs.git'
	platform :ios, '8.0'
	use_frameworks!
	
	pod 'Alamofire', '~> 1.2'

submodule 方式安装

	$ git submodule add https://github.com/Alamofire/Alamofire.git


##环境要求

Xcode 6

iOS 7.0+ / Mac OS X 10.9+

##Alamofire使用方法

###GET 请求

	Alamofire.request(.GET, "http://httpbin.org/get")

带参数

	Alamofire.request(.GET, "http://httpbin.org/get", parameters: ["foo": "bar"])

Response结果处理

	Alamofire.request(.GET, "http://httpbin.org/get", parameters: ["foo": "bar"])
	         .response { (request, response, data, error) in
	                     println(request)
	                     println(response)
	                     println(error)
	                   }

Response结果字符串处理

	Alamofire.request(.GET, "http://httpbin.org/get", parameters: ["foo": "bar"])
	         .responseString { (request, response, string, error) in
	                  println(string)
	         }

##HTTP 方法（Medthods）

Alamofire.Method enum 列表出在RFC 2616中定义的HTTP方法 §9:

	public enum Method: String {
	    case OPTIONS = "OPTIONS"
	    case GET = "GET"
	    case HEAD = "HEAD"
	    case POST = "POST"
	    case PUT = "PUT"
	    case PATCH = "PATCH"
	    case DELETE = "DELETE"
	    case TRACE = "TRACE"
	    case CONNECT = "CONNECT"
	}

这些值可以作为Alamofire.request请求的第一个参数.

	Alamofire.request(.POST, "http://httpbin.org/post")
	
	Alamofire.request(.PUT, "http://httpbin.org/put")
	
	Alamofire.request(.DELETE, "http://httpbin.org/delete")

POST请求

	let parameters = [
	    "foo": "bar",
	    "baz": ["a", 1],
	    "qux": [
	        "x": 1,
	        "y": 2,
	        "z": 3
	    ]
	]
	
	Alamofire.request(.POST, "http://httpbin.org/post", parameters: parameters)

发送以下HttpBody内容:

	foo=bar&baz[]=a&baz[]=1&qux[x]=1&qux[y]=2&qux[z]=3

Alamofire 使用Alamofire.ParameterEncoding可以支持URL query/URI form,JSON, PropertyList方式编码参数。

Parameter Encoding

	enum ParameterEncoding {
	    case URL
	    case JSON(options: NSJSONWritingOptions)
	    case PropertyList(format: NSPropertyListFormat,
	                      options: NSPropertyListWriteOptions)
	
	    func encode(request: NSURLRequest,
	                parameters: [String: AnyObject]?) ->
	                    (NSURLRequest, NSError?)
	    { ... }
	}

NSURLRequest方式编码参数

	let URL = NSURL(string: "http://httpbin.org/get")
	var request = NSURLRequest(URL: URL)
	
	let parameters = ["foo": "bar"]
	let encoding = Alamofire.ParameterEncoding.URL
	(request, _) = encoding.encode(request, parameters)

POST JSON格式数据

	Alamofire.request(.POST, "http://httpbin.org/post", parameters: parameters, encoding: .JSON(options: nil))
	         .responseJSON {(request, response, JSON, error) in
	            println(JSON)
         }

Response 方法

    response()
    responseString(encoding: NSStringEncoding)
    responseJSON(options: NSJSONReadingOptions)
    responsePropertyList(options: NSPropertyListReadOptions)
###上传（Uploading）

支持的类型

    File
    Data
    Stream
    Multipart (Coming Soon)

上传文件

	let fileURL = NSBundle.mainBundle()
                      .URLForResource("Default",
                                      withExtension: "png")

	Alamofire.upload(.POST, "http://httpbin.org/post", file: fileURL)

###上传进度

	Alamofire.upload(.POST, "http://httpbin.org/post", file: fileURL)
        .progress { (bytesWritten, totalBytesWritten, totalBytesExpectedToWrite) in
            println(totalBytesWritten)
        }
        .responseJSON { (request, response, JSON, error) in
            println(JSON)
        }

###下载
支持的类型

    Request
    Resume Data

下载文件

	Alamofire.download(.GET, "http://httpbin.org/stream/100", destination: { (temporaryURL, response) in
	    if let directoryURL = NSFileManager.defaultManager()
	                          .URLsForDirectory(.DocumentDirectory,
	                                            inDomains: .UserDomainMask)[0]
	                          as? NSURL {
	        let pathComponent = response.suggestedFilename
	
	        return directoryURL.URLByAppendingPathComponent(pathComponent)
	    }
	
	    return temporaryURL
	})

下载到默认路径

	let destination = Alamofire.Request.suggestedDownloadDestination(directory: .DocumentDirectory, domain: .UserDomainMask)
	
	Alamofire.download(.GET, "http://httpbin.org/stream/100", destination: destination)

下载进度

	Alamofire.download(.GET, "http://httpbin.org/stream/100", destination: destination)
	         .progress { (bytesRead, totalBytesRead, totalBytesExpectedToRead) in
	             println(totalBytesRead)
	         }
	         .response { (request, response, _, error) in
	             println(response)
	         }

###认证（Authentication）
支持以下几种认证

    HTTP Basic
    HTTP Digest
    Kerberos
    NTLM

Http basic认证

	let user = "user"
	let password = "password"
	
	Alamofire.request(.GET, "https://httpbin.org/basic-auth/\(user)/\(password)")
	    .authenticate(HTTPBasic: user, password: password)
	    .response {(request, response, _, error) in
	        println(response)
	        }

采用NSURLCredential&NSURLProtectionSpace方式认证

	let user = "user"
	let password = "password"
	
	let credential = NSURLCredential(user: user, password: password, persistence: .ForSession)
	let protectionSpace = NSURLProtectionSpace(host: "httpbin.org", port: 0, `protocol`: "https", realm: nil, authenticationMethod: NSURLAuthenticationMethodHTTPBasic)
	
	
	
	Alamofire.request(.GET, "https://httpbin.org/basic-auth/\(user)/\(password)")
	    .authenticate(usingCredential: credential, forProtectionSpace: protectionSpace)
	    .response {(request, response, _, error) in
	        println(response)
	}

###Printable

	let request = Alamofire.request(.GET, "http://httpbin.org/ip")
	
	println(request)
	// GET http://httpbin.org/ip (200)

###调试

    let request = Alamofire.request(.GET, "http://httpbin.org/get", parameters: ["foo": "bar"])

	debugPrintln(request)

###Output (cURL)

	$ curl -i \
	    -H "User-Agent: Alamofire" \
	    -H "Accept-Encoding: Accept-Encoding: gzip;q=1.0,compress;q=0.5" \
	    -H "Accept-Language: en;q=1.0,fr;q=0.9,de;q=0.8,zh-Hans;q=0.7,zh-Hant;q=0.6,ja;q=0.5" \
	    "http://httpbin.org/get?foo=bar"

更多的用法将会在接口文档中一一列出,敬请期待。

Alamofire与AFNetworking是同一个作者

>作者李刚是刚刚在线（www.superqq.com）站长，百度百家专栏作者
>iOS工程师非著名自媒体，微信公众号iOS开发：iOSDevTip运营者

![iOSDevTip](http://upload-images.jianshu.io/upload_images/624136-963198258f95cc10.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


