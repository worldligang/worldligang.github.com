---
layout: post
title: "NSFileManager文件操作的十个小功能"
date: 2015-07-24 00:14:03 +0800
comments: true
categories: iOS开发
keywords: 文件操作, iOS NSFileManager, NSFileManager, iOS开发, 刚刚在线
description: NSFileManager是一个单列类，也是一个文件管理器。可以通过NSFileManager创建文件夹、创建文件、写文件、读文件内容等等基本功能。

---
NSFileManager是一个单列类，也是一个文件管理器。可以通过NSFileManager创建文件夹、创建文件、写文件、读文件内容等等基本功能。

下面将介绍NSFileManager文件操作的十个小功能。我们在Documents里面进行举例，首先是获取Documents的路径。这个在[iOS开发之沙盒机制（SandBox)](http://www.superqq.com/blog/2015/07/20/ioskai-fa-zhi-sha-he-ji-zhi-%28sandbox/)已经详细讲解过了。获取Documents路径方法如下：

	- (NSString *)getDocumentsPath
	{
	    //获取Documents路径
	    NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
	    NSString *path = [paths objectAtIndex:0];
	    NSLog(@"path:%@", path);
	    return path;
	}
	
###创建文件夹
	
	-(void)createDirectory{
	    NSString *documentsPath =[self getDocumentsPath];
	    NSFileManager *fileManager = [NSFileManager defaultManager];
	    NSString *iOSDirectory = [documentsPath stringByAppendingPathComponent:@"iOS"];
	    BOOL isSuccess = [fileManager createDirectoryAtPath:iOSDirectory withIntermediateDirectories:YES attributes:nil error:nil];
	    if (isSuccess) {
	        NSLog(@"success");
	    } else {
	        NSLog(@"fail");
	    }
	}
	
###创建文件

	-(void)createFile{
	    NSString *documentsPath =[self getDocumentsPath];
	    NSFileManager *fileManager = [NSFileManager defaultManager];
	    NSString *iOSPath = [documentsPath stringByAppendingPathComponent:@"iOS.txt"];
	    BOOL isSuccess = [fileManager createFileAtPath:iOSPath contents:nil attributes:nil];
	    if (isSuccess) {
	        NSLog(@"success");
	    } else {
	        NSLog(@"fail");
	    }
	}
	
###写文件

	-(void)writeFile{
	    NSString *documentsPath =[self getDocumentsPath];
	    NSString *iOSPath = [documentsPath stringByAppendingPathComponent:@"iOS.txt"];
	    NSString *content = @"我要写数据啦";
	    BOOL isSuccess = [content writeToFile:iOSPath atomically:YES encoding:NSUTF8StringEncoding error:nil];
	    if (isSuccess) {
	        NSLog(@"write success");
	    } else {
	        NSLog(@"write fail");
	    }
	}
	
###读取文件内容

	-(void)readFileContent{
	    NSString *documentsPath =[self getDocumentsPath];
	    NSString *iOSPath = [documentsPath stringByAppendingPathComponent:@"iOS.txt"];
	    NSString *content = [NSString stringWithContentsOfFile:iOSPath encoding:NSUTF8StringEncoding error:nil];
	    NSLog(@"read success: %@",content);
	}
	
###判断文件是否存在

	- (BOOL)isSxistAtPath:(NSString *)filePath{
	    NSFileManager *fileManager = [NSFileManager defaultManager];
	    BOOL isExist = [fileManager fileExistsAtPath:filePath];
	    return isExist;
	}
	
###计算文件大小

	- (unsigned long long)fileSizeAtPath:(NSString *)filePath{
	    NSFileManager *fileManager = [NSFileManager defaultManager];
	    BOOL isExist = [fileManager fileExistsAtPath:filePath];
	    if (isExist){
	        unsigned long long fileSize = [[fileManager attributesOfItemAtPath:filePath error:nil] fileSize];
	        return fileSize;
	    } else {
	        NSLog(@"file is not exist");
	        return 0;
	    }
	}
	
###计算整个文件夹中所有文件大小

	- (unsigned long long)folderSizeAtPath:(NSString*)folderPath{
	    NSFileManager *fileManager = [NSFileManager defaultManager];
	    BOOL isExist = [fileManager fileExistsAtPath:folderPath];
	    if (isExist){
	        NSEnumerator *childFileEnumerator = [[fileManager subpathsAtPath:folderPath] objectEnumerator];
	        unsigned long long folderSize = 0;
	        NSString *fileName = @"";
	        while ((fileName = [childFileEnumerator nextObject]) != nil){
	            NSString* fileAbsolutePath = [folderPath stringByAppendingPathComponent:fileName];
	            folderSize += [self fileSizeAtPath:fileAbsolutePath];
	        }
	        return folderSize / (1024.0 * 1024.0);
	    } else {
	        NSLog(@"file is not exist");
	        return 0;
	    }
	}
	
###删除文件

	-(void)deleteFile{
	    NSString *documentsPath =[self getDocumentsPath];
	    NSFileManager *fileManager = [NSFileManager defaultManager];
	    NSString *iOSPath = [documentsPath stringByAppendingPathComponent:@"iOS.txt"];
	    BOOL isSuccess = [fileManager removeItemAtPath:iOSPath error:nil];
	    if (isSuccess) {
	        NSLog(@"delete success");
	    }else{
	        NSLog(@"delete fail");
	    }
	}
	
###移动文件

	- (void)moveFileName
	{
	    NSString *documentsPath =[self getDocumentsPath];
	    NSFileManager *fileManager = [NSFileManager defaultManager];
	    NSString *filePath = [documentsPath stringByAppendingPathComponent:@"iOS.txt"];
	    NSString *moveToPath = [documentsPath stringByAppendingPathComponent:@"iOS.txt"];
	    BOOL isSuccess = [fileManager moveItemAtPath:filePath toPath:moveToPath error:nil];
	    if (isSuccess) {
	        NSLog(@"rename success");
	    }else{
	        NSLog(@"rename fail");
	    }
	}
	
	
###重命名

	- (void)renameFileName
	{
	    //通过移动该文件对文件重命名
	    NSString *documentsPath =[self getDocumentsPath];
	    NSFileManager *fileManager = [NSFileManager defaultManager];
	    NSString *filePath = [documentsPath stringByAppendingPathComponent:@"iOS.txt"];
	    NSString *moveToPath = [documentsPath stringByAppendingPathComponent:@"rename.txt"];
	    BOOL isSuccess = [fileManager moveItemAtPath:filePath toPath:moveToPath error:nil];
	    if (isSuccess) {
	        NSLog(@"rename success");
	    }else{
	        NSLog(@"rename fail");
	    }
	}
	

以上文件操作代码都在[iOSStrongDemo](https://github.com/worldligang/iOSStrongDemo)，可以直接通过git拉下来进行测试。关于NSFileManager的用法还有很多，一起接续探索吧！

