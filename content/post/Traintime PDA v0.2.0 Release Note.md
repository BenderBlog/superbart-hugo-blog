+++
title = "Traintime PDA v0.2.0 发行简记"
slug = "Traintime PDA v0.2.0 Release Note"
description = "Traintime PDA v0.2.0 的介绍和技术相关"
date = "2023-08-16"
image = "https://legacy.superbart.top/picture/xdyou/homepage.jpg"
categories = [
    "Technology"
]
tags = [
    "Flutter",
    "编程",
]
+++

没想到很快我就发了 v0.2.0 版本，和 v0.1.0 版本相比，我感觉更多的是完善，和准备上架。

说是发行简记，实际上我要写很多的技术相关细节。

## 新功能介绍

1. 空闲教室查看功能，写起来比我想的简单。但是我是大摆子，我不知道真的会有人用嘛.png
2. 移除西电目录，使用电话本代替。点击对应卡片可以拨出电话。
3. 很多的 WebView 功能，比如报修啥的。这玩意主要可以水功能，还能对标其他产品。
4. 应某个工作室请求，我写了个双创需求大厅，希望各位能从上面更好地拉队友(别跟我一样啥奖都没有，QAQ)
5. 校园网感觉短期内不会有写的必要了，所以写进 WebView 了（溜）。
6. 顺利上架 F-Droid，然后貌似站点就给墙了？

## 关于接下来的任务

1. 物理实验查询，我目前不做实验了，所以可能得找人帮忙了()
2. 上架 iOS 商店。
3. iOS 和 Android 小部件，我需要进一步研究。

## 关于上架 iOS

目前我打算这个版本尝试申请 Testflight。据我所知，至少有三个组+两个人也在写这个东西，我无论如何也得打出去第一炮。

> 这里我无端想到了《东周列国春秋篇》电视剧里面的要离。
>   
> 中学学过“专诸刺王僚，要离刺庆忌”，不知道咋回事。看了电视剧才知道，他为了出名，壮士断腕。吴王阖闾说：“你是要名，还是要家？”结果就不必说了……
> 
> 我现在也有点那啥，我为了这玩意，已经砸进去很多了。我这辈子都没一次性花这么多钱，现在我不上架，真对不住那么钱了。但上架了话，真的会有那么多人用嘛？
> 
> 我这玩意，真要跟电表，跟其他原生，可以说是被爆打。也许就真的只是“开源+第一个上架”？开源这年头算毛线的优势？
>   
> 写这个程序有一段时间，我一直在想这件事，不过现在释然了。

## 关于技术相关

点击这个可以查看[v0.1.0 的技术相关](https://superbart.top/p/traintime-pda-v0.1.0-release-note.html)。

### Webview Cookie 相关

想在 Flutter 使用 Webview ，你可以使用两个插件：[webview_flutter](https://pub.dev/packages/webview_flutter) 和 [flutter_inappwebview](https://pub.dev/packages/flutter_inappwebview)。前者是官方开发，功能基础；后者是第三方开发，功能强大。我为了保证简洁，使用的是前者。

关于插件，网上很多资料都是很老的，我参考了这位的文章：[在 Flutter 中使用 webview_flutter 4.0](https://juejin.cn/post/7196698315835260984)，其中最有用的是第三篇，讲怎么用 Cookie 的。我的程序是这样写的：
1. WebView 页面中，接受要前往的网站和获取 Cookie 的网站。
2. 在 initState 状态下，初始化 Webview 的 CookieManager 和 Controller。WebView 的控制器可以控制加载，页面前进和回去。
3. 在 didChangedDepencies 状态下，根据获取 Cookie 的网站，从 Dio 的 CookieJar 中获取 Cookie。然后控制器请求对应网站。

具体代码在[这里](https://github.com/BenderBlog/traintime_pda/blob/main/lib/page/homepage/toolbox/webview.dart)。

最后，这个玩意貌似在 iOS 平台下有 bug，Cookie 死活加不进去，我已经提 bug 了:-P

### 上架 F-Droid 平台

F-Droid 有两个好：
 - 开源的东西多，就是好
 - 目前我程序在安卓平台唯一可以“自动更新”的方式

Flutter 程序上架，除了官方的，可以参考这位的[上传指南](https://friesi23.github.io/flutter/android/fdroid/appstore/2023/06/08/submitting-your-flutter-app-to-fdroid.html)。我想补充两点————可重复构建，分开架构构建：

F-Droid 的可重复构建，对我而言，最主要的就是使分发都带上我的签名。这就需要保证构建元数据需要你签名的 sha256 摘要，和一个可供对照的构建（在我这里就是我在 Github Action 上面的构建）。

分开架构构建，就是按照手机架构（arm64，arm32，x86）来构建分发包。这个东西，貌似每个架构的版本构建号还不一样。当时写构建元数据的时候，写到弃疗。他们 F-Droid 的审核人好好，帮我写了T_T

我的上架过程可以看看[这个链接](https://gitlab.com/fdroid/fdroiddata/-/merge_requests/13537)，合并请求后四天，真正上架。你们可以从这里[点进链接下载](https://f-droid.org/zh_Hans/packages/io.github.benderblog.traintime_pda/)。

另外说为啥来这里上架，我这软件确实是自由软件。还有，国内上架需要这个那个的，感觉好麻烦，而且已经有电表了，再上架一个感觉也吸引不了多少。

### 双创需求大厅相关

这个东西，主要是使用了 Dart 3 的最新语言功能：Records。详情[看这个文章](https://juejin.cn/post/7233067863500849209).

我没记错，go 好像能一次性返回两个值。一开始我感觉很神奇，然后相似的东西就降临到 Flutter 了。说回来，如果没有这个东西，我会考虑 Pair / List，大不了写个 class 。

双创需求大厅本质上跟找工作网站差不多，都得有个 Popup 来选择职位状况。这个东西的服务器筛选工作，是需要两个东西：一个 String 传大致分类，一个字符串数组传输 tags。我选择这俩东西的部件是写在外面的，需要返回数据的话，我直接写 `(String, List<String>)` 就可以了。读取的这些数据的话，可以通过 `$1` 或 `$2` 来读取。

不过这玩意现在只有五个数据，以后会不会变多呢？也许我能通过这个，说一波我程序和xxx合作？

### 课程表代码变化

为了将来看得方便，我使用了 InheritedWidget 部件来存储课程表数据。课程表数据相关，请看我之前写的东西。Flutter 有组件树和渲染树，我理解不多，但我知道 InheritedWidget 组件相当于存有数据的树根，在其底下的孩子都可以读取这里的数据。这样就能跨部件共享数据了。实际上这个东西我们早就用过了，当时那篇介绍文章使用的是 `MediaQuery.of(context).size` 来举例。

关于周次选择轴/滚动锁和页面控制器，貌似 InhheritedWidget 不喜欢变化很大的数据，还是在组件里初始化啥的，我只好写在了别的类。为了保证子部件好监听，我使用了 ChangeNotifier 让他们监听。不过貌似只用在了解锁最顶部的锁:-p

还有个问题，就是最顶部的初始滚动。目前刚打开的情况下，如果周次很靠后，可能会出现弹的情况。这个要解决，我得保证屏幕变化的时候，我能保证屏幕宽度的数据能让监听器有所察觉。这块……反正我是有点迷糊，不过感觉无伤大碍(希望)。

## 结语

这就是 v0.2.0 的发行简记，感谢阅读。