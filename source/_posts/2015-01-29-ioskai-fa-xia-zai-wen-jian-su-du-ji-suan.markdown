---
layout: post
title: "iOS开发下载文件速度计算"
date: 2015-01-29 10:40:38 +0800
comments: true
categories: iOS开发
keywords: AFNetworking, 速度, Cocoapods, 速度计算, 下载, NSByteCountFormatter, AFHTTPRequestOperation, iOS, iOS开发, cocoapods, 个人博客, 刚刚在线
description: "iOS开发下载文件速度计算"

---

当我们写下载界面的时候，需要向用户展示每秒下载多少KB，这个时候就需要计算速度。如下：

<img src="http://mmbiz.qpic.cn/mmbiz/8RTSPr4mlykfVEtYcoETGMzCaNgWWfvvjjxmJYIj9ibjNHHmn7f1oOTgEbcTstj44G2EjdqRjeMxSqdJfVaAibPA/640" /></a>

我用的是AFNetworking来做下载的，我们拿AFHTTPRequestOperation来举列，AFHTTPRequestOperation中有个方法：

    - (void)setDownloadProgressBlock:(void (^)(NSUInteger bytesRead, long long totalBytesRead, long long totalBytesExpectedToRead))block

这个方法读取导数据之后通过block回调。计算速度就是在这个block里面完成的。

需要定义一个下载对象：DownTask。DownTask需要有的属性：totalRead（一秒读取的数据）、speed（速度）、date（记录上一秒计算之后的时间）。注意：这里的一秒不是严格意义上的一秒，有可能大于一秒，我们计算的是平均速度，所以不会误差太大。

            //计算一秒中的速度
            downTask.totalRead += bytesRead;

            //获取当前时间
            NSDate *currentDate = [NSDate date];

            //当前时间和上一秒时间做对比，大于等于一秒就去计算
            if ([currentDate timeIntervalSinceDate:downTask.date] >= 1) {
                //时间差
                double time = [currentDate timeIntervalSinceDate:downTask.date];

                //计算速度
                long long speed = downTask.total/time;

                //把速度转成KB或M
                downTask.speed = [downTask formatByteCount:speed];

                //维护变量，将计算过的清零
                downTask.totalRead = 0.0;

                //维护变量，记录这次计算的时间

                downTask.date = currentDate;
                    

其中， downTask.speed = [downTask formatByteCount:speed];是用来将字节转化成我们需要的KB或M。用到的是：NSByteCountFormatter，具体实现如下：

    - (NSString*)formatByteCount:(long long)size
    {
        return [NSByteCountFormatter stringFromByteCount:size countStyle:NSByteCountFormatterCountStyleFile];
    }

这个方法是放在下载对象DownTask里面的。

以上就是我计算下载速度的方法。希望能给你带来帮助。

更多iOS开发相关技术请关注iOS开发微信公众号 iOS开发 ：

	iOSDevTip

