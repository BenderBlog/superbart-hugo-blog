+++
title = "Traintime PDA v0.4.0 发行简记"
slug = "Traintime PDA v0.4.0 Release Note"
description = "Traintime PDA 0.4.0 的介绍和技术相关"
date = "2023-10-01"
image = "https://legacy.superbart.top/picture/xdyou/homepage.jpg"
categories = [
    "Technology"
]
tags = [
    "Flutter",
    "编程",
]
+++

# Traintime PDA 0.3.0 & 0.4.0 发行简记

本来我是不想现在就上架 App Store，但是电表突然上架了。虽然目前功能少，但着实打了一惊，我也顾不上我软件的不成熟，也上架了。看来大家还是很认可我的软件，所以感觉可以。我也很感谢很多帮我的人，无论是画吉祥物的，还是帮我发传单的，给我 UI 设计提出建议的。

之前我好像说过学校“揭榜”的事情，这玩意确实有点用，就是在面试时候问项目背景的时候，至少能扯到学校:-P

说是发行简记，实际上我要写很多的技术相关细节。

## 新功能

不包括 bug 修复。

### v0.3.0
1. iOS 版本添加吉祥物，绘画者是 [Ray](https://ray.al/)
2. 应用内信息，会有开发者发出的学校/社团/应用信息

### v0.4.0
1. 物理实验查看功能
2. 现在必须填写密码才能看体育打卡记录。

## 关于接下来的任务

可以加入我程序的 [Testflight](https://testflight.apple.com/join/pLKe5B4q) 来尝鲜。

### v0.4.x 计划
1. 课表添加输出为 icalender 格式，方便 iOS 导入日历。
2. XDU Planet 买个新服务器运行起来。
3. 优化掉一些不需要控制器的页面，减少加载失败概率。
4. 把体育打卡成绩加回来。
5. 新知道个查签到次数的脚本，打算集成。


### v1.0.0 计划
1. 优化首页 UI 的设计。
2. 集成考试，物理实验到课表内，进行统一的日程展示。(大功能，容易鸽子)

### 将来计划
1. 桌面小部件。
2. 研究生版本打算写个网页服务器，输入学号密码获取 icalender 课表。

## 关于技术相关

### 关于物理实验，乱码处理和 Dio 转换器

我们学校目前的物理实验服务器使用的是 2005 年的 ASP 技术，重点在 2005 年。实际上技术差点也没啥，但是有两点属实离谱：

1. 所有的信息都是用 GB2312 编码的。
2. 传回的 Cookie 有中文字符的字段。

其中第二点是最离谱的。

对于 Dart 底层的默认 UTF-16 String 来说，这俩点属实头疼。

#### 乱码处理

乱码实际上很常见，常知道的锟斤拷梗就跟这个相关。毕竟汉字跟英文一样，在电脑底层都是需要用二进制编码来表示的。简体中文汉字有两个主要编码：

1. 国标码：一个用于编码汉字和一些日韩字符的国家标准，主要有 GB2312，GBK，GB18030 三个标准，呈现继承与发展（向下兼容）的特性。请查看[这个链接](https://zhuanlan.zhihu.com/p/453675608)来搞清国标码(GBK)相关。Windows 默认就是这个编码。国标码是定长编码，基本使用两个字节(16 位二进制位)来表示一个汉字。
2. UTF 编码：国际上有个统一码联盟，他们负责给全世界所有的字符编码，称为 Unicode。很早他们就支持了中日韩三个语言字符的编码（由于文字特性，中日韩字符在他们的体系中，在一个分区）。Unicode 只是规定了字符对应的二进制表示，但实际使用，位数过长而且浪费很多，所以实际使用只能继续缩短，使用更短的变长编码，称为 UTF。UTF 分成很多版本，一般代表了最短编码位数是多少。Linux / Mac + 互联网数据一般都是用这个编码。详情可以查看[这个链接](https://zhuanlan.zhihu.com/p/427488961)。

说到变长编码知识，计算机组成会讲汇编命令是如何编码的，那里会讲的。

很明显，如果用 UTF 编码解析国标码，绝对会解析出不正常的数据。大巧不巧，Dart 语言的 String 本质上是一个 UTF-16 编码的序列。于是问题就产生了。

国标码是定长编码，而 UTF 是变长编码，很显然是基本没法兼容的。不兼容还好，在我的实践中，用 UTF 编码先编码回二进制信息，然后用国标码解码信息，大概率是无法得到正确的数据。

所以我目前程序中，需要让网络库不能用 Dart 的 String 来解码我的数据，我需要一个支持国标码的解码库。

#### Dart / Flutter 的 GBK 解码库

这个实际上有两种：

1. 流行方案：使用 UTF 和 GBK 的码表一一对应，方便转换。这个方式对平台很灵活，缺点需要让我程序增大 500k 左右，而且这种方式在执行时候也会有些慢。
2. 调用系统的解码接口来解码信息，我使用的是这个方案。但是缺点也很明显，如果没有对目标系统适配，解码就很难办。

最终我使用的是这个库：[charset_converter](https://pub.dev/packages/charset_converter)。它目前能 Windows，Android，iOS 三个系统的转码，而且使用很方便。他支持很多编码，但我主要用国标码。

#### 关于 Dio 的转换器

Dio 的网络请求使用的是过滤器流水线模式：

> HTTP 请求 -> 若干拦截器 -> 转换器 -> Dart 底层实现或系统网络实现
> 响应的二进制码 -> 转换器 -> 若干拦截器 -> HTTP 响应

拦截器一般处理 Cookie，判断响应码之类。目前 Dio 的拦截器不支持异步方法。

转换器 Transformer 是一个二进制码和 HTTP 请求响应结构互相转化的桥梁。默认的 Transformer 是解码后用来对 body 进行判断的。由于我上面提到，不能用 UTF 先编码再解码，所以我定制了一个 Transformer，称为 `ExperimentDioTransformer`。在一些基本对 Body 的二进制解析后，直接用 GBK 解码库来返回数据。学校物理实验服务器都是返回的网页，所以这么写没啥问题。

#### 关于 Cookie 有中文字符

可以看看[我在 Dio 开发仓库提出的问题](https://github.com/cfug/dio/issues/1959)。

Cookie 的官方规范，是仅允许一部分 ASCII 码作为合法字符的，Dart 核心库的 Cookie 实现严格遵照这个规范。但是令我哭笑不得的是，咱学校物理实验服务器传回的 Cookie 包含中文字符，就是这个用户的名字。加上 GBK 导致的编码，最后的结果自然就是报错，扔出“错误编码异常”。

人官方严格按照标准，无可厚非。我为了这个玩意折腾了很长时间，直到最后，有个人告诉我，那个 Cookie 给服务器传任何值都可以，我无语了......

### 关于应用内信息的分发机制

借鉴了[这个项目](https://github.com/xeonds/xdu-planet)。接下来，根据我的“服务器”和借鉴项目的 Github Action 配置文件，我给大家做一个大致的部署过程讲解。

#### 借鉴项目的 Action

Go 版本的 XDU Planet，本质上就是 RSS 处理转 json，然后用 gin 开服务器端口。这个项目使用 Github Action 来每小时更新，然后更新成一个 json 文件，最后搞到 Github Page。

这个项目有三个分支：主代码，配置文件，部署分支。发布流程大致如下：

1. Action 环境初始化，获取代码（Checkout）。
2. 对代码进行构建，对于这个项目，就是构建 go 代码和 vue 代码。
3. 使用 go 生成的可执行文件，生成 json 文件。
4. 上传生成的网页和 json 到部署分支，然后在部署分支的基础上部署 Github Action。

#### 我的“通知服务器”

可以看看[这个链接](https://github.com/BenderBlog/traintime_pda_backend)。核心技术就是用 [Miller](https://github.com/johnkerl/miller) 来将 csv 转换成 json，然后用 Github Action 推到 Page 服务。同样的，这个项目有两个分支：

1. main 分支：存储 csv 文件和 Github Action 配置文件。
2. depoly 分支：存储需要通过 Github Page 发布的 json 文件。

发布流程和上面的差不多：

1. Action 环境初始化，获取代码（Checkout）。
2. 将 csv 转换为 json 文件。
4. 上传 json 到部署分支，然后在 depoly 分支的基础上部署 Github Action。

### 关于一点点 iOS 开屏娘的事情

这个玩意主要用到了 XCode 的界面设计工具。长这样：

![](https://legacy.superbart.top/picture/xdyou/XDYou_XCode_LaunchImage.png)

Apple Store 上架需要程序有个开屏图，我于是找个人画个漫画。画家顺便画个手绘板的图标，风格对应了。

这个玩意我当时搞了接近一个下午才搞成，大部分时间在摸索这玩意到底咋用，小部分时间在看各个手机屏幕大小情况下的排版状况。最终我摸索出这样的排版：

1. 上面人脸下面图标，在一个中轴线上。
2. 人脸大小写死，因为我不知道如何动态调整图片大小:P 图标比例写死 1:1。
3. 人脸中心在 Y 轴中心上面(减去) 80px 处，图标在 Y 轴下面(加上) 200 像素处。

## Dreams Never End by New Order (former Joy Division)

[歌曲的 MV 点此观看](https://www.bilibili.com/video/BV1Sy4y1E7Uy)

```
My promise could be your fiend   
A given end to your dreams  
A simple movement or rhyme  
Could be the smallest of signs  
We'll never know what they are or care  
In it's escapable view  
There's no escape so few in fear  
Give in a changing value  

To be given your sight  
Hid in a long peaceful night  
A nervous bride for your eyes  
A fractured smile that soon dies  
A love that's wrong from your life and soul  
A savage mine had begun  
Hello, farewell to your love and soul  
Hello, farewell to your soul  

Now I know what those hands would do  
No looking back now, we're pushing through  
We'll change these feelings, we'll taste and see  
But never guess how the him would scream  
But never guess how the him would scream  
But never guess how the him would scream   
```

Yours and us legacy continues, no matter what happens...
