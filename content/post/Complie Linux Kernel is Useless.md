+++
title = "自己编译 Linux 内核，好像一点用都没有？"
slug = "Complie Linux Kernel is Useless"
description = "同志们没事不要折腾"
date = "2022-02-11"
image = "https://s2.loli.net/2022/08/01/HuqXCyzwJA3N2kE.jpg"
categories = [
    "Technology"
]
tags = [
    "Linux",
    "编译"
]
+++

## 目录
* 为什么我要自己编译内核～ Linux 内核的多元化
* 为啥自己编译没用～性能对比和优劣势对比
* 如何加速内核编译～使用 modprobed-db 精简驱动模块
* 我到底配置了啥～给大家看看我改过的内核配置
* 结尾

## 为什么我要自己编译内核～ Linux 内核的多元化
很简单，下学期我有门课，叫“操作系统”，据说需要编译内核。实际上我之前编译过，但是我没有接触过设置，这回想看看我能设置啥。  
不过我最讨厌学习了，所以接下来的才是真正原因233  
我玩《黑山起源》，玩起来很卡。游戏设置当然是调了，但根据我之前压制视频，我觉得是内核没有把我的核显和 CPU 压榨干净(我的电脑是轻薄本)。之前看过很多帖子，说用了特制内核，跑起来能快一些。  
这里，我提到了“特制内核”。因为 Linux 内核是开源的，自然，有人魔改了很多版本。这里介绍四个版本：
1. 长期支持版(LTS)，为了稳定而优化的版本，相当于 Windows 的 LTSC 版。一般出现在 CentOS 和 Ubuntu 上面。
2. linux-zen，为了桌面电脑而进行过性能优化。我日用这个版本。
3. linux-hardened，为了系统安全而优化的版本。
4. linux-libre，为了代码的绝对自由而砍掉了很多驱动。

当然，如果你是为了应付操作系统实验，我建议你还是使用原版吧。首先，网上教程丰富，其次，代码简单易得。

## 为啥自己编译没用～性能对比
鉴于我编译内核，最主要的出发点是加速游戏运行，自然我得提供这方面的数据了。  
我的自制内核，基于 linux-zen 内核，精简了很多没必要的驱动，以及在电脑管理模块强制使用性能模式，并根据我的处理器型号(AMD Ryzen 4750U)，使用了"Zen 2"性能优化。  
以下跑分均在我的电脑上进行，型号是 Thinkpad T14 ，系统是最新的 Arch Linux ，在接电情况下进行。  
首先是GeekBench(下称GB)的跑分成绩：  
<table>
	<tr>
		<th rowspan="2">测试次数</th>
		<th colspan="3">原版 zen 内核</th>
		<th colspan="3">自制 zen 内核</th>
	</tr>
	<tr>
		<th>单核成绩</th>
		<th>多核成绩</th>
		<th>GB 数据库编号</th>
		<th>单核成绩</th>
		<th>多核成绩</th>
		<th>GB 数据库编号</th>
	</tr>
	<tr>
		<td>1</td>
		<td>1204</td>
		<td>5038</td>
		<td>12522767</td>
		<td>1209</td>
		<td>5112</td>
		<td>12523274</td>
	</tr>
	<tr>
		<td>2</td>
		<td>1191</td>
		<td>5090</td>
		<td>12522823</td>
		<td>1209</td>
		<td>5119</td>
		<td>12523312</td>
	</tr>
	<tr>
		<td>3</td>
		<td>1214</td>
		<td>5087</td>
		<td>12522819</td>
		<td>1208</td>
		<td>5109</td>
		<td>12522353</td>
	</tr>
	<tr>
		<td>4</td>
		<td>1206</td>
		<td>5070</td>
		<td>12522915</td>
		<td>1207</td>
		<td>5126</td>
		<td>12523397</td>
	</tr>
	<tr>
		<td>5</td>
		<td>1215</td>
		<td>5086</td>
		<td>12522951</td>
		<td>1204</td>
		<td>5126</td>
		<td>12523431</td>
	</tr>
	<tr>
		<td>6</td>
		<td>1217</td>
		<td>5092</td>
		<td>12522998</td>
		<td>1212</td>
		<td>5098</td>
		<td>12523485</td>
	</tr>
	<tr>
		<td>平均成绩</td>
		<td>1208</td>
		<td>5077</td>
		<td>-</td>
		<td>1208</td>
		<td>5115</td>
		<td>-</td>
	</tr>	
</table>

接下来是《半条命2：失落的海岸线》(与《黑山起源》同为起源引擎)的跑分成绩，单位是 FPS：
<table>
	<tr>
		<th>测试次数</th>
		<th>原版 zen 内核</th>
		<th>自制 zen 内核</th>
	</tr>
	<tr>
		<td>1</td>
		<td>155.31</td>
		<td>151.91</td>
	</tr>
	<tr>
		<td>2</td>
		<td>137.70</td>
		<td>139.08</td>
	</tr>
	<tr>
		<td>3</td>
		<td>137.63</td>
		<td>141.41</td>
	</tr>
	<tr>
		<td>平均FPS</td>
		<td>143.5</td>
		<td>144.1</td>
	</tr>
</table>

由上可见，虽然自编译内核相较原版内核，有一定的性能提升，但是提升幅度不大。而我还发现，使用强制性能模式会导致电脑风扇长时间运行，CPU 过热现象明显。而在新内核下运行《黑山起源》，我觉得流畅度有些微提升，至少没有之前那么卡了。但是我高度怀疑这是某种安慰剂效应。  
所以，自行编译内核并没有达到我的需求。但这不意味着我白搞了一通，至少编译内核速度快了。
补充说明：我当时玩的是《黑山起源》的 Linux 版本，那个版本被很多的 ProtonDB 用户评为垃圾水平，因为在 AMDGPU 上会有贴图故障，而且不太更新。详情看这个。
好了，进入我这篇文章的宝藏部分捏。

## 如何加速内核编译～使用 modprobed-db 精简驱动模块
[modprobed-db](https://github.com/graysky2/modprobed-db) 是一个 bash 脚本，他能侦测你系统目前所使用的模块，并记录下来。在编译内核的时候，程序只会编译我们使用过的驱动模块，加速编译速度，减少内核体积。

>**注意**：使用这个软件，可能会精简驱动过头，导致使用不便。请各位打算使用前，最好稍微了解一下内核配置选项。本人仅在 Arch Linux 下运行过这个软件，如果你用的是 Ubuntu 等系统，使用有问题的话，请跟我说一下。

### 过一下编译内核的一般步骤
1. 电脑装好编译环境，一般包括 gcc，make 等。Arch Linux 是要安装上 `base-devel` 软件包组和 `gcc` 。
2. 你需要拖下来最新稳定版的内核源代码，然后进入源代码文件夹：
```bash
$ git clone https://mirrors.tuna.tsinghua.edu.cn/git/linux-stable.git # 这里使用了清华镜像
$ cd linux-stable
```
3. 使用以下任意一个命令，配置内核参数：
```bash
$ make nconfig # 命令行界面配置
$ make xconfig # 图形化界面配置(使用 QT )
```
>**注意**: 很多教程是用make config配置内核，本人不推荐。界面太原始就算了，在 Arch Wiki 上面被标记为"被 nconfig 取代"

![nconfig长这样](https://legacy.superbart.top/picture/Compile%20Linux%20Kernel%20is%20Useless/Power%20Preformance.jpg)

4. 使用 `make -j$n` 命令编译，这里 `$n` 代表你电脑/虚拟机的核心数。
5. 使用以下两个命令来安装内核：
```bash
$ sudo make module_install # 安装内核模块
$ sudo make install # 安装内核本身
```
6. 重启到新内核，如果没有的话，查看系统引导器设置。

### 使用 modprobed-db 精简内核 
1. 获取 modprobed-db 软件。Arch Linux 用户可以使用 AUR 直接安装 `modprobed-db` 软件包。如果不是的话，根据该软件 Github 所介绍：
```bash
$ git clone https://github.com/graysky2/modprobed-db
$ cd modprobed-db
$ make
$ sudo make install
```
2. 获取目前你电脑正在挂载的模块：
```bash
$ modprobed-db # 初始化软件
$ modprobed-db store # 获取目前运行模块并保存在一个数据库中
$ modprobed-db list # 列出存在数据库里面，电脑运行过的的内核模块记录
$ modprobed-db debug # 列出目前运行系统模块和数据库记录的异同
```
3. 在编译内核的时候，配置内核参数部分，执行这个命令来关掉不需要的模块编译开关。然后编译安装即可。
```
$ make LSMOD=$HOME/.config/modprobed.db localmodconfig
```

### 使用提示
如果你是实机运行的话，务必把所有你要使用的设备都使用上。这里我翻译一下[ Arch Wiki 的原文](https://wiki.archlinux.org/title/Modprobed-db#Populating_the_database)：
* 挂载上所有需要用到的文件系统
* 接上所有需要用到的可移动媒体，比如U盘，光驱等
* 以上选项包括挂载 ISO 文件，这个涉及到 loop 模块和 isofs 模块
* 使用电脑上的所有设备，例如网卡，输入设备，电脑摄像头，移动设备等
* 使用电脑上的所有应用程序，有些程序是需要特定内核模块来运行的，比如虚拟机
* 在不同版本/特制的内核上运行 modprobed-db，也许会录入一些其他内核没有的模块

我当时没有插上我的光驱，就运行了这个，结果新内核没法读我的光驱:-P

## 我到底配置了啥～给大家看看我改过的内核配置
我上面说过，使用 modprobed-db 的前提是对内核配置有一定了解，至少需要看到选项的时候，脑瓜不疼。(如果你是应付操作系统实验，我看[我们学校的操作系统资料](https://github.com/LevickCG/Happy-SE-in-XDU/tree/master/OS)和[小梦哥哥的实验总结](https://moefactory.com/3041.moe)的步骤，我觉得你要是在虚拟机下直接搞，应该没有问题)  
所以，我来给大家看一下我的内核配置吧，给大家看看我改了什么。这里我用 `make nconfig` 配置。  
第一个选项是总体选项，是包括了内核压缩，特定版本号之类的信息。请看 xmgg 的吧。  
![默认界面](https://s2.loli.net/2022/08/01/jwCd34vDs9EqmiV.jpg)
这是默认界面，配置程序给了我们一些选项。下面的功能键中，F2可以查看配置选项的详细信息，F9可以搜索配置选项。  
![处理器类型和特性](https://s2.loli.net/2022/08/01/MsVvJqoDXEr18RQ.jpg)
这个地方是配置处理器相关信息的。我这里把很多因特尔处理器的独家特性给删掉了，然后处理器优化强制设置为Zen 2。其他方面的有任务调度之类，我没有动，因为不懂。  
![插入电源管理特性](https://s2.loli.net/2022/08/01/MvdYJQ4K5ANZc3D.jpg)
这个地方配置电源管理，我开启了休眠和睡眠，然后将CPU频率调整设置为"性能"。  
![文件系统选项](https://s2.loli.net/2022/08/01/gVZfTyXWjeBE1s4.jpg)
这个地方配置文件系统支持，是精简内核的重中之重，也是一个坑。如果精简过头，可能插个U盘读不出来。尤其注意CD文件系统和DOS文件系统选项。  
![驱动配置](https://s2.loli.net/2022/08/01/269QYoNTSOxhRad.jpg)
这个地方配置驱动选项，基本上编译内核，大部分时间都是在编译驱动。所以，这个地方我们可以大开杀戒。不过千万不要要把你需要用到的驱动给去了。  
其他方面，诸如支持32位可执行程序，内核安全算法，调试选项等，我就不说啦。

## 结尾
虽然自己编译内核，没有使游戏性能有很大提升😶  
但是我由此得到了提升内核编译的一个途径，这要将来节省时间~~卷过别人~~不就很方便了吗🥰  
实际上我还给内核打上了[中文补丁](https://github.com/zhmars/cjktty-patches)，不过网上很多教程，我就不在这说了。给大家个[链接](https://zhuanlan.zhihu.com/p/375460344)看看吧。没记错命令是 `patch -Np1 < 补丁文件` 。  
我还使用了 Arch Linux 的包管理工具，让整个过程更简单。具体看[这个](https://wiki.archlinux.org/title/Kernel/Arch_Build_System)。

## 推荐阅读
[Arch Wiki提供的编译内核指南(多系统适用哦)](https://wiki.archlinux.org/title/Kernel/Traditional_compilation)  
[小梦哥哥教大家操作系统实验啦](https://moefactory.com/3041.moe)
