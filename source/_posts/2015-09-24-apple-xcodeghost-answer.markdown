---
layout: post
title: 有关 XcodeGhost 的问题和解答
date: 2015-09-24 00:09:03 +0800
comments: true
categories: 推荐
keywords: XcodeGhost, 苹果, 苹果官方, iOS开发, 刚刚在线
description: XcodeGhost我们一直建议开发者使用由我们提供的免费、安全的工具，包括 Xcode，从而确保他们为 App Store 的用户创造出安全的 app。一些开发者下载了已被恶意软件感染的盗版 Xcode，由此开发的 app 也同样受到感染。

---

**我听说了由 XcodeGhost 开发的恶意 app — 这是怎么回事？**

我们一直建议开发者使用由我们提供的免费、安全的工具，包括 Xcode，从而确保他们为 App Store 的用户创造出安全的 app。一些开发者下载了已被恶意软件感染的盗版 Xcode，由此开发的 app 也同样受到感染。

Apple 特意使用诸如 Gatekeeper 等技术，以防止安装从非 App Store 渠道下载的应用程序，和 / 或安装包括 Xcode 在内的未签名的应用程序。当开发者为了能安装类似 XcodeGhost 等恶意程序时，这些保护措施会被刻意地禁用。

作为 Apple 向开发者提供的业界先进工具之一，以下措施可以确保软件未被篡改：

    Xcode app 有 Apple 的代码签名。
    从 Mac App Store 下载 Xcode 时，开发者的电脑系统自动对 Xcode 的代码签名会进行检查和验证。
    从 Apple Developer Program 网站下载 Xcode 时，只要 Gatekeeper 没有被禁用，默认开发者的电脑系统对 Xcode 代码签名自动进行检查和验证。

###为什么开发者会不顾用户的安全下载盗版软件？

为了更快下载我们的开发者工具，开发者有时会从其他非 Apple 站点搜寻。

###这会对我有什么影响吗？如何得知我的设备是否受到了影响？

我们目前没有任何信息表明这些恶意软件与任何恶意事件相关，也没有信息表明这些软件被使用在传播任何个人身份信息的用途上。

我们目前没有看到任何客户个人身份信息受到影响，而且代码无法通过用户身份请求来获取 iCloud 或其他服务的密码。

只要一经发现这些 app 有可能通过恶意代码开发，我们就对其进行下架处理。开发者们正在快速更新他们的 app，以便用户使用。

<!--more-->


恶意代码只能提供一些基本信息，比如 app 和一般系统信息。

###从 Apps Store 下载 app 是否安全？

我们已将由该盗版软件开发的 apps 从 App Store 中撤下，并拦截了通过该恶意软件开发的新 app 进入 App Store。

我们正与开发者紧密协作，以确保受到影响的 app 尽快回到 App Store 供用户使用。

我们将在支持页面上列出受此影响的前 25 个 apps，方便用户验证他们是否已将这些 app 更新到了最新版本。

用户还将会收到更多信息，以便了解他们下载的某 app 是否会存在问题。一旦开发者更新了他们的 app，用户可以通过在设备上运行更新解决存在的问题。

我们正努力让中国的开发者可以用更快的速度下载 Xcode 测试版本。开发者也可以通过 developer.apple.com 列出的步骤来验证他们的 Xcode 是否被篡改过。

##XcodeGhost Q&A

**I’ve heard about malicious apps created by XcodeGhost — what does this mean?**

We always recommend developers using the free, secure tools we provide them — including Xcode — to ensure they’re creating the most secure apps for App Store customers. Some developers downloaded counterfeit versions of Xcode that have been infected with malware and created apps that were just as infected.

Apple incorporates technologies like Gatekeeper expressly to prevent non-App Store and/or unsigned versions of programs, including Xcode, from being installed. Those protections had to have been deliberately disabled by the developer for something like XcodeGhost to successfully install.

As part of providing developers the industry's most advanced tools, Apple provides developers the following checks to ensure software is untampered:

    The Xcode app is code-signed by Apple.
    When you download Xcode from the Mac App Store the code signature for Xcode is automatically checked and validated by your system.
    When you download Xcode from the Apple Developer Program web site, the code signature for Xcode is automatically checked and validated by your system by default as long as Gatekeeper is not disabled.

###Why would a developer put customers at risk by downloading counterfeit software?

Sometimes developers search for our tools on other, non-Apple sites in an effort to find faster downloads of developer tools.

###How does this affect me? How do I know if my device has been compromised?

We have no information to suggest that the malware has been used to do anything malicious or that this exploit would have delivered any personally identifiable information had it been used.

We’re not aware of personally identifiable customer data being impacted and the code also did not have the ability to request customer credentials to gain iCloud and other service passwords.

As soon as we recognized these apps were using potentially malicious code we took them down. Developers are quickly updating their apps for users.

Malicious code could only have been able to deliver some general information such as the apps and general system information.

###Is it safe for me to download apps from App Store?

We have removed the apps from the App Store that we know have been created with this counterfeit software and are blocking submissions of new apps that contain this malware from entering the App Store.

We’re working closely with developers to get impacted apps back on the App Store as quickly as possible for customers to enjoy.

A list of the top 25 most popular apps impacted will be listed soon so users can easily verify if they have downloaded the latest versions of these apps. After the top 25 impacted apps, the number of impacted users drops significantly.

Customers will be receiving more information letting them know if they’ve downloaded an app/apps that could have been compromised. Once a developer updates their app, that will fix the issue on the user’s device once they apply that update.

We’re working to make it faster for developers in China to download Xcode betas. To verify that their version of Xcode has not been altered, they can take the following steps posted at <developer.apple.com>.

文章来自：http://www.apple.com/cn/xcodeghost/