+++
title = "我的一加8T刷机失败记录"
slug = "Failure on Flash OnePlus 8T"
description = "我的第一篇博文，刷机的痛苦体验"
date = "2021-02-03"
image = "https://s2.loli.net/2022/08/01/53U9OBFlswnKCrV.png"
categories = [
    "Technology"
]
tags = [
    "刷机"
]
+++


注意：本文不是详细教程，只是我的痛苦体验罢了，文章末尾我会给链接的。  

最近我的 Nokia(HMD) 7 Plus 的充电口彻底没有办法充电了，所以我妈给我买了一台一加8T:-)  

到手的第一件事嘛......一定是解锁呀。辛亏一加的解锁是相当容易的，开发者模式中开启“OEM 解锁”，然后 adb reboot bootloader 进入fastboot，再运行 fastboot oem unlock，手机上音量加减选择解锁，电源键选择即可。  

然后我没想到的部分还是来了，鉴于本手机刚刚发布，很多第三方系统还没有完全适配。我最想用的Lineage没有官方，而crDroid是Beta品质，这都是我后来才发现的:-P  

不过官方论坛上有教程，那自然是得一顿操作啦。可惜呢......  

![这个是我在Windows下的第二次失败，第一次是在Linux下的](https://s2.loli.net/2022/08/01/53U9OBFlswnKCrV.png)

开始是Linux系统下失败，然而在Windows下也失败了。我暂且认为是开发者用的是256G的，可我的是128G的原因吧。  

无论如何，我必须得救砖了。然而救砖软件是Windows独占，我就启动了该死的Windows虚拟机，手机完全关机并同时按住上下键，映射到虚拟机（设备名字开头是高通啥的）。结果报错，自动检测DDR失败。我想是因为虚拟机映射有问题，所以我直接重装了该死的Windows（我上期翻译的视频字幕文件没了，其他的因为备份了，还在）来救砖。有一次电脑不认，我按住了上下键和电源键来强制开机。  

![注意红箭头指示的地方，一定先要选上！否则救砖失败，手机变成假砖](https://s2.loli.net/2022/08/01/oSlVrIB4PWghOYt.png)

救砖成功了，我又在Windows下尝试了好几遍fastboot，全部失败:-P 我看等成熟的卡刷可以实现的时候再说吧。
