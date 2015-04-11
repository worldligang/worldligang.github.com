---

layout: post
title: "iOS开发大众点评URL签名生成规则和SHA-1编码"
date: 2014-12-13 13:52:29 +0800
comments: true
categories: iOS开发
keywords: iOS, URL签名, 大众点评API, iOS开发, gcd, cocoapods, 队列, 个人博客, 刚刚在线
description: "iOS开发大众点评URL签名生成规则和SHA-1编码"

---

</br>
URL签名生成规则

所有[大众点评API](http://www.superqq.com/blog/2014/12/13/ioskai-fa-da-zhong-dian-ping-urlqian-ming-sheng-cheng-gui-ze-he-sha-1bian-ma/)的有效访问URL包括以下三个部分：
1. 资源访问路径，如/v1/deal/find_deals;
2. 请求参数：即API对应所需的参数名和参数值param=value，多个请求参数间用&连接
   如deal_id=1-85462&appkey=00000；
3. 签名串，由签名算法生成

 
签名算法如下：
1. 对除appkey以外的所有请求参数进行字典升序排列；
2. 将以上排序后的参数表进行字符串连接，如key1value1key2value2key3value3...keyNvalueN；
3. 将app key作为前缀，将app secret作为后缀，对该字符串进行SHA-1计算，并转换成16进制编码；
4. 转换为全大写形式后即获得签名串

 
签名串获得后，将其作为sign参数附加到对应的URL中，即可正常访问API。

 	
 	注意：请保证HTTP请求数据编码务必为UTF-8格式，URL也务必为UTF-8编码格式。
 	

具体调用示例请参见<a href="https://github.com/dianping/dianping-open-sdk"target="_blank"title="刚刚在线">https://github.com/dianping/dianping-open-sdk</a>  



	+ (NSDictionary *)parseQueryString:(NSString *)query {
	    NSMutableDictionary *dict = [[NSMutableDictionary alloc] initWithCapacity:6];
	    NSArray *pairs = [query componentsSeparatedByString:@"&"];
	    
	    for (NSString *pair in pairs) {
	        NSArray *elements = [pair componentsSeparatedByString:@"="];
	        
	        if ([elements count] <= 1) {
	            return nil;
	        }
	        
	        NSString *key = [[elements objectAtIndex:0] stringByReplacingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
	        NSString *val = [[elements objectAtIndex:1] stringByReplacingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
	        
	        [dict setObject:val forKey:key];
	    }
	    return dict;
	}



	+ (NSString *)serializeURL:(NSString *)baseURL params:(NSDictionary *)params
	{
	    NSURL* parsedURL = [NSURL URLWithString:[baseURL stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding]];
	    NSMutableDictionary *paramsDic = [NSMutableDictionary dictionaryWithDictionary:[self parseQueryString:[parsedURL query]]];
	    if (params) {
	        [paramsDic setValuesForKeysWithDictionary:params];
	    }
	    
	    NSMutableString *signString = [NSMutableString stringWithString:DIANPING_APP_KEY];
	    NSMutableString *paramsString = [NSMutableString stringWithFormat:@"appkey=%@", DIANPING_APP_KEY];
	    NSArray *sortedKeys = [[paramsDic allKeys] sortedArrayUsingSelector: @selector(compare:)];
	    for (NSString *key in sortedKeys) {
	        [signString appendFormat:@"%@%@", key, [paramsDic objectForKey:key]];
	        [paramsString appendFormat:@"&%@=%@", key, [paramsDic objectForKey:key]];
	    }
	    [signString appendString:DIANPING_APP_Secret];
	    unsigned char digest[CC_SHA1_DIGEST_LENGTH];
	    NSData *stringBytes = [signString dataUsingEncoding: NSUTF8StringEncoding];
	    if (CC_SHA1([stringBytes bytes], (long)[stringBytes length], digest)) {
	        /* SHA-1 hash has been calculated and stored in 'digest'. */
	        NSMutableString *digestString = [NSMutableString stringWithCapacity:CC_SHA1_DIGEST_LENGTH];
	        for (int i=0; i<CC_SHA1_DIGEST_LENGTH; i++) {
	            unsigned char aChar = digest[i];
	            [digestString appendFormat:@"%02X", aChar];
	        }
	        [paramsString appendFormat:@"&sign=%@", [digestString uppercaseString]];
	        return [NSString stringWithFormat:@"%@://%@%@?%@", [parsedURL scheme], [parsedURL host], [parsedURL path], [paramsString stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding]];
	    } else {
	        return nil;
	    }
	}
	

直接调用这个方法即可签名成功，+ (NSString *)serializeURL:(NSString *)baseURL params:(NSDictionary *)params

	NSString *url = [LGClient serializeURL:string params:nil];
	
	//string是大众点评的需要请求的接口

这个url就可以用来请求大众点评数据了

我的微信公众号：

	iOSDevTip

我也会将大家分享的方法发到iOS开发微信公众账号：iOSDevTip 里面，分享给更多iOS开发者。
