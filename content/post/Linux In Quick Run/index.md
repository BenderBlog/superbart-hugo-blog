+++
title = "快速逃离Linux指南"
description = "一些Linux相关的使用技巧，帮助大家用完Linux赶紧逃"
date = "2022-01-25"
image = "https://benderblog.github.io/picture/Linux%20In%20Quick%20Run/Cover(Copyright%20Yumiko%20Chiba).webp"
categories = [
    "Technology"
]
tags = [
    "Linux",
    "虚拟机",
    "镜像"
]
+++

看完了，搞定完操作系统实验，快跑！  
注意：我不可能把在互联网上随便找到的教程再写一遍，我觉得很啰嗦，所以请各位多多使用互联网。  
如果你是大佬，好好沉下心来帮帮小白，行吗。  

## 目录

<ol>
    <li>虚拟机的相关</li>
    <li>镜像使用</li>
    <li>好奇怪的桌面</li>
    <li>文件相关</li>
    <li>命令行的基本使(chao)用(xi)</li>
    <li>Linux系统安全教育</li>
    <li>还有没说到的，上网查资料/优雅地问问题</li>
    <li>为什么我不推荐大家使用Linux</li>
</ol>


## 虚拟机相关

相信各位是被老师的某些需求，才知道有个操作系统叫Linux，才想安装的吧。而各位肯定不想在自己唯一的实机上安装，估计你们都连系统都没装过，会碰到一堆问题:-P
所以虚拟机是一个更好的选择，它是模拟了一个类似于你电脑的环境。你在里面怎么折腾，只要不出格，基本上不会对你电脑里的其他东西有影响。  
这里我不会教大家如何设置一个虚拟机，我给大家一些便于使用的指南。  

### 增强功能

一般安装完系统，你需要在虚拟机里的系统安装虚拟机的增强功能。安装完增强功能有啥好处呢？

* 窗口缩放自动化，你没有必要盯着640x480的上古分辨率了。
* 相当于给虚拟机里的系统打上了驱动。最直观的，画面更流畅了。
* 虚拟机和宿主机可以共享一个剪切板，抄点命令代码更方便了。
* 虚拟机和宿主机可以互相分享文件了。

这里，给VirtualBox用户来个建议，一定要装VirtualBox软件的增强模块！上网找一下Oracle Extension Pack了解一下吧。  
好了，如何安装捏？我知道网上可以找到一大堆的安装教程，但我突然间想多写一些，想让大家少走不必要的弯路。

<table>
<thead>
<tr>
<th>虚拟机软件</th>
<th>虚拟机里的系统</th>
<th>安装方式</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" rowspan="2">VMWare Workstation</td>
<td align="center">Windows</td>
<td align="center">从VM选项找到”安装VMWare Tools”选项，然后安装</td>
</tr>
<tr>
<td align="center">Linux</td>
<td align="center">从你系统的软件库中安装open-vm-tools</td>
</tr>
<tr>
<td align="center" rowspan="3">Oracle VirtualBox</td>
<td align="center">Windows</td>
<td align="center">下载VBoxGuestAddtions.iso，虚拟机挂载安装</td>
</tr>
<tr>
<td align="center">Red Hat系Linux发行版</td>
<td align="center">同Windows安装方式</td>
</tr>
<tr>
<td align="center">其他的Linux发行版</td>
<td align="center">从你系统的软件库中安装virtualbox-guest-utils</td>
</tr>
</tbody>
</table>


顺便你还可以知道虚拟镜像挂载的知识呢。

### 硬件虚拟化

虚拟化毕竟是模拟了一个电脑环境，这就好比某些双面人一样，心累啊。不过电脑没心没肺，没有道德真空，这不挺好的吗:-)  
话说现在的CPU，都支持辅助虚拟化技术。这玩意简单来说，可以让虚拟机直接调用CPU的某些指令，让电脑更加轻松地进行虚拟化。要是没有这个，就真的只能靠软件模拟运行了，效率能把你逼疯。就像某些双面人一样，表面装好人，不过要没人陪衬，迟早装不下去的。  
这个特性，Intel的叫VT-x，AMD的叫AMD-V。相信我，没有开这个玩意，大概率你的虚拟机会很卡，甚至有你的虚拟机可能都无法启动:-P  
所以在这里，我要给大家的建议是：

* 查看自己的BIOS设置，看看有没有开虚拟化设置(记住这个单词：Virtualization)
* 看看你的虚拟机CPU设置，有没有开虚拟化设置(一般选项里都有VM-T/AMD-V字符串)

### 共享文件夹

实际上前面我提到的增强工具，有一个文件互相拖拽功能，不过个人认为，超级难用。一般来说，虚拟机需要访问宿主机文件的话，我更倾向于使用共享文件夹功能。这个功能本质上，就是把宿主机的一个文件夹通过某种虚拟机内部的网络共享方式，让虚拟机访问。  
至于怎么用，给你们一些指南，具体怎么做，请询问可爱的互联网姐姐:-)  

![这个是VMWare虚拟机下，配置共享文件的方式](https://benderblog.github.io/picture/Linux%20In%20Quick%20Run/VMWareFileShare.png)

![这个是VirtualBox虚拟机下，配置共享文件的方式](https://benderblog.github.io/picture/Linux%20In%20Quick%20Run/VirtualBoxFileShare.webp)

设置的时候，尽量勾选上自动挂载/开机挂载，这样能省下很多的事情。  
还有一件事，读写权限也要搞明白，我前面说不出格就没事，是因为虚拟机和宿主机本来是隔离的，现在有了一个口子互相通信了，万一你在虚拟机搞了危险操作，极有可能你那些珍贵的东西就要遭殃了。(实际上共享剪贴板也有风险，但比这个小多了)

### 系统快照功能

我先给大家讲两个案例，一个是电脑行装系统，一个是Windows的系统还原。  
电脑行装系统，喜欢用Ghost。他们提前预制出一个系统环境，然后用Ghost软件保存下来。组装完电脑后，他们把这个模板“扣进”新机器，系统就装完了。  
Windows有个功能，叫系统还原。当你的电脑出现问题的时候，还原一下就好了。  
系统快照在某种意义上，就是上面那俩的集合。它的功能，就是把虚拟机的状态(包括磁盘状态，硬盘状态)保存下来，类似于一个模板环境。然后你在虚拟机里面操作，发现系统坏了，直接拿之前的快照还原一下就行了。这比Windows的系统还原还好用呢，真的是一键还原。  
具体怎么用，互联网姐姐比我更清楚呢，我给你们俩地图吧：

![这个是VMWare虚拟机的系统快照功能](https://benderblog.github.io/picture/Linux%20In%20Quick%20Run/VMwareSnapshot.jpg)

![这个是VirtualBox虚拟机的系统快照功能](https://benderblog.github.io/picture/Linux%20In%20Quick%20Run/VirtualBoxSnapshot.jpg)

既然我们的目的是为了一个干净的环境，方便还原。我建议各位存两个快照：一个在系统安装完成之后，一个是在你干活之前。  
对了，快照本身也是需要更新的。因为虚拟机里面的系统是需要更新的，所以干活之前的快照一定要更新。至于最干净的，系统完成之后的镜像，一般是为了在虚拟机彻底没法用的时候，搞的救命稻草。  
还有一件事，快照回退的时候，在快照生成时间之后的所有东西，设置都将消失！所以你有啥必须要保存的东西，看看上面的共享文件夹功能。

## 镜像使用

相信大家遇到过这样的情况：

* 需要下载一个软件，兴致冲冲跑到官网下载，结果发现下载速度好慢啊:-(
* 你需要用pip搞点数学计算，结果下载的时间够你出门晒太阳了
* 你想去搞点其他的资料，然而就是上不去

没关系，各大高校和互联网公司已经帮你下好了，你从他们那里取就行了。

### 先告知你

* 清华大学镜像站：https://mirrors.tuna.tsinghua.edu.cn
* 中科大镜像站：https://mirrors.ustc.edu.cn
* 如果你是我校友的话(仅校内服务)：https://linux.xidian.edu.cn/mirrors

镜像站一般会给你很多的帮助指南，一定要充分利用。镜像站的用途还是很多的，以下只是一些示例。

### 加速Linux系统更新

鉴于这是Linux指南，不提Linux有点不太合适。  
Linux系统的优点之一，就是软件更新比Windows舒服。但默认更新一般是很慢的，因为要走国外的服务器。所以说，更改系统的软件源地址就很有必要了。

* Ubuntu简便方法：设置里面有个选项，叫”软件与更新”，从那里修改。
* Fedora/CentOS简便方法：一般需要看镜像源的文档，开命令行复制粘贴命令。
* Arch Linux/Manjaro：编辑/etc/pacman.d/mirrorlist文件。
* Debian/Ubuntu：编辑/etc/apt/sources.list文件。
* Red Hat系列：编辑/etc/yum.repos.d下面的一堆repo文件。本人超级不建议编辑，能烦死。

对了，既然说到了软件源，这里预告一下，第五章讲命令行的时候我会细说这个的。

### 加速github的clone

也不知为何，我们要从github下面拉下一个文件，总是好慢啊。幸亏现在有很多的镜像站来帮助我们快速下载。  
这个我就直接扔俩地址，以及一个命令：  
镜像1：https://hub.fastgit.xyz  
镜像2：https://github.com.cnpmjs.org  
命令：这个命令能让git访问github的时候，访问镜像。

```bash
git config --global url."镜像网址".insteadOf https://github.com
```

### 加速下载软件

现在要下载啥软件，都喜欢找最近的镜像地点，加速你的下载。可就怕这玩意不好使用，你别说，我下载Eclipse IDE的时候就遇到过。  
当然，幸运的话，镜像源都会给你备份好了。自己探索吧，我觉得没必要多说了233

![页面上提供了Java(OpenJDK),AOSP(安卓系统源代码)的镜像 下面还有VLC，OBS,Wireshark,FDroid等软件的镜像](https://benderblog.github.io/picture/Linux%20In%20Quick%20Run/TsinghuaMirror.jpg)

## 好奇怪的桌面

如果安装完了系统，进去发现系统有点不一样，但感觉上还能用，那你们真幸运:-)我六年前开始用Linux的时候，还不是这样呢。  
但如果你发现，电脑操作不太一样了，或者说，你的Linux和他的不一样。那么，你就要先了解以下东西了。

### 桌面居然是一个独立的软件？

这点和Windows很不一样，Windows的图形化功能是集成到内核的，Linux不是这样。这也解释了为啥Windows图形化一崩溃就蓝屏了。  
具体来说，Linux本身只是一个内核，在其上运行着很多程序，图形化界面(桌面环境)只是其中一个。  
要细说的话，我们得扯一下历史了(欢迎大家进入工程概论睡觉模式)：  

![SuperBart超级抽象画工时间2333](https://benderblog.github.io/picture/Linux%20In%20Quick%20Run/OldTimeWithTerminal.jpg)

上世纪七八十年代的电脑，都是需要用终端机来使用的。终端机连接到远端的主机，并进行操作。现在有些东西，还有这个的影子呢，比如你远程你买的云服务器。插一句，C语言的stdio头文件，全称叫标准输入输出(STanDard Input Output)，也是对应了这个结构。输入在当时，就是终端机的键盘，输出在当时，就是终端机的屏幕。当然现在，分别对应的是你的键盘和屏幕了。  
Linux的图形化程序叫Xorg，也是这样的结构。这里给张图片。  

![根据维基百科X协议页面画的，不一定准确](https://benderblog.github.io/picture/Linux%20In%20Quick%20Run/XStructure.jpg)

你看，是不是有点终端机和主机的感觉呢？前面三个负责处理一些内部事情，比如接受进程状态，检测输入之类。然后XORG服务器将绘制信号传给XORG客户端，然后经由窗口管理器之类的东西，把窗口送到你的屏幕上。他的过程比Windows那样的直接绘制要复杂一些，但是十分灵活。  
在Linux中，有很多的桌面环境。建议大家看一下自己系统使用的桌面环境，以后出现问题的话，会很有用。下面介绍一些著名的桌面环境，以及我认为的特点:
<table>
<thead>
<tr>
<th align="center">名称</th>
<th align="center">官网</th>
<th align="left">优点</th>
<th align="left">缺点</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center">KDE</td>
<td align="center">kde.org</td>
<td align="left">围绕KDE开发的软件很多，界面比Windows 11还好看，配置方便</td>
<td align="left">体积庞大</td>
</tr>
<tr>
<td align="center">Deepin</td>
<td align="center">deepin.org</td>
<td align="left">界面华丽，使用简单，开发单位有国家赞助</td>
<td align="left">3d加速之类的东西不太适合虚拟机使用</td>
</tr>
<tr>
<td align="center">GNOME</td>
<td align="center">gnome.org</td>
<td align="left">个性化能力强，围绕其生态的软件多</td>
<td align="left">默认界面使用十分反人类，用起来十分不稳定</td>
</tr>
<tr>
<td align="center">MATE</td>
<td align="center">mate.org</td>
<td align="left">界面直观，软件丰富，基于GNOME还没反人类时期的代码</td>
<td align="left">可能界面有点老土，默认上下都有任务栏</td>
</tr>
<tr>
<td align="center">XFCE</td>
<td align="center">xfce.org</td>
<td align="left">省资源，但是软件绝对够用，小耗子很可爱</td>
<td align="left">界面十分老土，个人认为得自己配置一下</td>
</tr>
<tr>
<td align="center">LXDE</td>
<td align="center">lxde.org</td>
<td align="left">十分省资源</td>
<td align="left">配置起来相当麻烦，软件之类得自己找</td>
</tr>
</tbody>
</table>

当然，听我的一家之词，肯定是不够的。建议各位上网找一下相关图片，了解一下。  

![这个小恐龙比那个恶心人的辅导员恐龙好多了，我不理解为啥我们假期那么短](https://benderblog.github.io/picture/Linux%20In%20Quick%20Run/DragonOnMyDesktop.jpg)

### 我的中文输入法呢？

相信有人装完系统，发现你的系统没有中文输入法，中文输入不了。  
Linux的输入法跟Windows是有区别的，Linux上的输入法是一个框架，在框架中，具体的输入法才能运行。Linux上面有两个框架，一个叫Fcitx，一个叫ibus。接下来，我会给大家一些关于输入法的提示。  
首先是fcitx(小企鹅输入法)。这个输入法的用途还是很广泛的，而且插件功能强大。我用的最多，也最想给大家推荐。具体安装我这里不会细说，给点提示吧:  

* 一般来说，你需要安装一些针对QT和GTK的相容性插件。如果你发现输入不了的话，可能这是你问题的一个切入点。
* 目前Fcitx分为两个版本，一个是第四版，一个是第五版。现在推荐大家使用第五版，功能更多，开发更活跃。
* 关于默认的拼音输入法，有两个插件一定要激活：一个是云拼音插件，一个是词库插件。云拼音插件可以从百度的服务器上面得到你输入拼音的预测，词库插件可以获取搜狗拼音的词库。

然后是ibus。这个是GNOME的默认输入框架，所以用GNOME的同志们，不要再装fcitx了。这个输入法我用的不多，所以这里谈的不多，请进入设置里的相关选项进行设置。  

不过我必须插一句，不要使用默认的拼音输入法实现！去你的软件源找有没有ibus-libpinyin或者ibus-sunpinyin，这俩更好用。

### Linux上面有Dev-C++吗？

没有，Dev-C++是纯Windows程序。但是Linux上面有更好用的。

* 小熊猫Dev-C++，QT版的Dev-C++，该怎么用不用我多说了吧。(这不是原版Dev-C++，不保证你的软件仓库有)
* Geany，用起来和Dev-C++差不多，构建单个文件的时候很舒服的。
* CodeBlock，我们CPP语言老师用的是这个IDE。
* Kate，KDE桌面环境默认编辑器，个人习惯使用这个编辑配置文件。OI-Wiki有个指南，可以看看。(这个软件有Windows版)
* Gedit，Gnome桌面环境的默认编辑器。稍微配置一下，就能一键编译了。CSDN上面一堆教程呢。
* VSCode，大名鼎鼎，无需多言。而且在Linux上配置更方便了呢。

插一句啊，在Linux编程前，一定要看看你的系统有没有编译器！你安装gcc或者clang了吗？

## 文件相关

粗略略用起来，好像没啥奇怪的。但当你想找C盘D盘的时候，诶，跑哪里去了？  
你发现文件路径中，'/'用的好多啊，而且有好多三个字母的目录，有点高大上。  
你还发现文件属性里面没有"隐藏"了，这又是搞哪门子？

### 没有明显的分区概念

知道各位脑子里充满了C盘，D盘之类的。他们泾渭分明，基本上要没啥事的话，真的是鸡犬相闻，老死不相往来。但如果我告诉你，分区之间可以关系紧密，甚至成为了一棵树呢？  
来看看这张图吧，这就是我电脑Linux的分区结构了。

![手绘的更有温度，懂不懂啊](https://benderblog.github.io/picture/Linux%20In%20Quick%20Run/MyLinuxTree.jpg)

你看到了吗，任何文件都是衍生自一棵树，他的名字叫做根，他的目的也是为了耕种这些文件。这些文件在这个根的勤劳耕种下，努力地繁育系统这个大家庭……(看不懂的去看《十日谈》或者去听Genesis的Cinema Show)  
为什么说Linux的分区不明显呢？分区是硬件上的概念，客观存在的。但是Linux中，分区之间的关系是非常紧密的。即使/usr目录在一个分区，/boot在另一个分区，/单独一个分区，但只要有/维系这棵树，他们之间的互相访问，就好像在一个分区一样，这样，分区的概念就不明显了。  

![看分区的命令是sudo fdisk -l，/boot一个分区，/一个分区，还有16GB的交换空间(虚拟内存)](https://benderblog.github.io/picture/Linux%20In%20Quick%20Run/SDDPartition.jpg)

有心人注意到了，我写了一个"在内存中的文件"。这个是Linux内核把系统和硬件的信息，通过文件的形式给大家呈现了出来。这个方面，建议大家了解一下Linux/Unix下硬件映射为文件，“一切皆文件”的思想。  
对于mac用户，你们可以打开终端，看看你们的根目录。

### 隐藏文件和配置文件

在Linux中，隐藏文件的标志和Windows的不一样。只要你在文件名前面搞个`.`就行了，就这样。  
那么，什么情况下我们会看到隐藏文件呢？来看看我的电脑吧。

![左面的不显示隐藏文件，右面的显示隐藏文件](https://benderblog.github.io/picture/Linux%20In%20Quick%20Run/HiddenFileComparison.jpg)

好吧，你看到了很多的隐藏文件。这里面我先告诉你，大多数是配置文件。为啥要告诉你呢？要不然没法往下写了(尴尬)  
Linux软件的配置文件，大多集中地放在以下目录中:

* /etc 这个是系统级别的软件配置文件所在
* $HOME/.config 这个是在你的家目录(/home/你的用户名)里面的软件配置文件所在
* $HOME/.vkquake 这个是在家目录里，雷神之锤游戏的配置文件和数据包相关(有其他程序是话，请类比)

配置文件有啥可说的呢？Linux大多数应用都是依靠配置文件，而不是图形化配置工具，来修改设定的。而且，一般通过配置文件，你可以对这个软件的使用有初步的印象，因为很多的配置文件都写满了注释。实际上，前面我们修改软件源的时候，我们就已经修改系统的配置文件了。

## 命令行的基本使(chao)用(xi)

在Linux，你要想玩的high，就得接触命令行。对于某些在Windows经常搞cmd的人，估计会更熟悉些吧。  
但如果你不熟悉命令行，相信你的外语和程序上机都好好学了吧，这也不是难事。  
而且大家不是更喜欢CyberPorn吗2333

### 程序设计课复习：程序与参数

各位应该在C语言程序设计中，学到了如何通过命令行输入参数，而不是先把程序执行了，再输入数据。你们当时肯定输入的是这个：

```
int main(int argc, char* argv[])
```

其中第一个参数argc(argument count)，是你输入的参数数量。第二个参数argv(argument vector)，存放的是你输入的参数字符串。举个例子，前面我们提到要搞软件包管理。在Ubuntu下，你搜索软件包(举个例子，gcc编译器)的时候，你输入的是：

```
apt search gcc
```

这样，你输入了三个参数，一个是apt，一个是search，一个是gcc。这样的话，argc的数值是3，而argv里面存储的，则是那三个参数的字符串了。这里我建议各位自己编写一个和下面程序类似的程序。看看输出结果。
```C
#include<stdio.h>
int main(int argc, char* argv[]){
	for (int i = 0; i < argc; ++i){
		printf("%s\n",argv[i]);
	}
	return 0;
}
```

### 命令行程序举例：一句话编译C语言单文件

各位目前编程，除了某些大佬之外，肯定是依靠Dev-Cpp之类的程序来编译运行吧。这里我想给大家把那些程序的外表给去掉，给大家看看如何编译一个程序吧。  
像Dev-Cpp那样的，可以编辑代码并编译执行的程序，叫做集成开发环境(IDE)。IDE要想编译程序，需要编译器，这个配置过vscode的人会更清楚。接下来，我们只依靠编译器，编译上面的示例程序。  
这里我使用gcc编译器。咱先把上面的示例程序写下来，保存成'argc.c'文件。然后在保存这个文件的目录下，打开终端，通过以下命令编译运行。  

```bash
gcc argc.c -o argc && ./argc Unforeseen Consequence
```

如果执行没有问题的话，程序将会输出

```
./argc
Unforeseen
Consequence
```

好的，程序运行成功了，执行符合预期。这个命令我也该跟大家解释一下了：

* `gcc` 是编译器程序的名称，后面跟参数`–help`可以查看其使用指南
* `argc.c` 是需要编译的源代码文件，是gcc程序的参数
* `-o` 是gcc的参数，表示要将编译后的结果输出到哪个文件中，后面的argc是-o的参数
* `&&` 是bash命令解释器的一个特殊符号，表示在前面的命令完成后，执行后面的命令
* `./argc` 是即将执行的程序名称，`./`表示我们需要在当前目录下寻找该程序
* 后面的两个单词是argc程序的参数，也是G-Man对万斯父女说过的话

如果大家一时看不明白，很正常。我这里只是想通过这种方式，让大家对命令行程序有一个了解。  
如果想更多了解的话，建议大家了解一下bash的基本用法。最后给大家一道思考题：系统是怎样找到程序的位置呢？

### 软件包管理

前面我说镜像的时候，我说我会在这里细讲的。个人认为，这个是使用频率最高的命令行程序了。  
在Windows下，各位要用软件的时候，都会找渠道下载安装程序，然后安装吧。这种方式个人认为，十分麻烦，而且不安全。麻烦在于，你得满世界去找安装程序，有些小众程序还得去各种犄角旮旯网站去找。不安全在于，有些渠道很黑心，一不小心就给你来个2345流氓大礼包。要是下到了病毒，那就更好玩了:-P  
而Linux系统，普遍都有配套的软件库，可以很方便地给你们提供很多的软件。基本上咱们编程需要的东西，都给你准备好了。当然，要是这个程序找不到的话，如果那个软件给Linux适配的话，那就把上面的步骤走一遍吧:-(  
接下来，我给大家准备了一些命令，免得大家上网找了。

<table>
<thead>
<tr>
<th align="center">系统类型</th>
<th align="center">安装程序</th>
<th align="center">卸载程序</th>
<th align="center">更新程序</th>
<th align="center">更新系统</th>
<th align="center">搜索程序</th>
<th align="center">图形化工具</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center">Debian/Ubuntu</td>
<td align="center">apt-get install</td>
<td align="center">apt-get remove</td>
<td align="center">apt update &amp;&amp; apt upgrade</td>
<td align="center">apt update &amp;&amp; apt dist-upgrade</td>
<td align="center">apt-cache search</td>
<td align="center">synaptic</td>
</tr>
<tr>
<td align="center">Fedora/CentOS</td>
<td align="center">dnf install</td>
<td align="center">dnf remove</td>
<td align="center">dnf update</td>
<td align="center">dnf distro-sync</td>
<td align="center">dnf search</td>
<td align="center">-</td>
</tr>
<tr>
<td align="center">OpenSUSE</td>
<td align="center">zypper in</td>
<td align="center">zypper rm</td>
<td align="center">zypper up</td>
<td align="center">zypper dup</td>
<td align="center">zypper se</td>
<td align="center">YaST软件包管理工具</td>
</tr>
<tr>
<td align="center">Arch Linux/Manjaro</td>
<td align="center">pacman -S</td>
<td align="center">pacman -R</td>
<td align="center">pacman -Syu</td>
<td align="center">pacman -Syu</td>
<td align="center">pacman -Ss</td>
<td align="center">pamac/octopi</td>
</tr>
</tbody>
</table>


对了，用GNOME环境的同志们，你们的电脑上面应该有个"软件"应用，那个玩意也挺方便的。KDE下面有个Apper，也还行。  
还有一件事，软件库是可以扩展的，比如Fedora的RPMFusion，Archlinux的AUR，需要的话，可以上网了解一下。

## Linux系统安全教育

在阅读这一章之前，先把超人的座右铭读一下：能力越大，责任越大。  
Linux给你的权限是相当大的，鉴于很多人在Windows下，不一定能对系统权限有很深的认识，我不太想让大家因为网上的某些垃圾命令/恶意软件而搞得心情不愉快。所以这里，我简单说几句句。

### sudo和最高权限用户

各位在互联网上寻找到的命令，有一些前面带着`sudo`，或是`#`字符。这都意味着，这个命令需要使用最高用户权限(Linux叫root账户)来执行。  
在Windows下，有管理员账户(Administrator)。相信大家感受不深，因为各位的电脑默认都是这个账户。当你需要安装应用程序的时候，有个窗口弹出来，让你同意运行。这个情况下，系统就需要让你动用管理员用户权限了，因为你要更改系统设置，修改系统文件啊。Linux也是这样，当你需要安装软件的时候，你需要提权了。  
Linux的最高权限用户和Windows的管理员有很大不同。Windows的管理员权限在某种意义上，算是一种丞相的位置，虽然权力相当大了，但上面还有个SYSTEM账户，掌管所有权力。Linux的最高权限用户可谓是一人之下，万人之上了。你可以访问所有文件，修改所有设定，甚至一句话就可以自杀:-P  
这就要引出下一个话题了……

### Linux也有病毒

很多人说，Linux相较于Windows更安全，而且没有病毒。这个话是不完全正确的。  
先说错误的部分：

* Linux内核和上面的软件，和Windows一样，会有漏洞。虽然修复十分频繁，但毕竟洞在那里，很多人都会来插的。
* 由于Windows在普通人中间的使用量相当大，攻击者会花很多心思寻找Windows的漏洞，然后编写病毒攻击。Linux和Mac方面的病毒相比，就少了很多。
* Android系统基于Linux开发，然而为啥天天有人随便下载东西，然后手机被锁住了呢？

好吧，看上去，也不是那么美好啊。那么，正确的部分又在那里呢？  
在Linux下，调用最高管理权限的门槛很高。多数情况下，你在Windows中，默认就是管理员账户，UAC(提权时候的提醒)也近乎于摆设。而Linux的话，用户账户默认是没有最高权限访问权的。而当你提权的时候，往往需要把你加入提权组(一般叫wheel)，执行前输入密码。要是不提权的话，你只能操作你家目录里面的东西。而最高权限用户，默认是禁用的，只有当你给其设置密码的时候，才能使用。这种近似于一刀切的管理方式保证了Linux的安全。但如果你们提权了，稀里糊涂地从网上随便贴个危险命令，那就出大事了，比如说：

* `sudo rm -rf /*` 臭名昭著的自杀命令
* `:(){:|:&};:` 可理解为不停调用自己，把电脑卡死
* `whatever-command > /dev/sdaX` 直接用该命令的输出覆写到磁盘上，你的硬盘毁了

对于Linux而言，不要执行来路不明的程序，也是适用的。  
以上说到的，都算是Linux的“病毒”了。希望大家使用的时候一定要小心。

## 还有没说到的，上网查资料/优雅地问问题

我前面说过，这里给的东西，都是一些抛砖引玉的东西。如果没有你需要的，首先，我深感歉意:-(  
去互联网看看，或者找其他大佬吧，他们一定比我博学多了，不过普遍喜欢使用狗头:-P  
(我看到有人经常发狗头的时候，会过敏，我不知道他是不是在嘲讽我)

### 提高英语水平

这很关键！因为系统输出的东西都是英文的，而且你目前接触到的所有互联网资源，尤其是跟Linux相关的资源，都是英语的。实在看不懂，多用谷歌翻译吧。  

### 查看报错输出

报错输出是查错的时候，非常有用的资源。通过阅读它，你能很快明白问题的根源，并进行针对性的上网，搜索解决方案。这里给个例子。

![他想装一个软件，卡在这里了](https://benderblog.github.io/picture/Linux%20In%20Quick%20Run/ProblemCheck.jpg)

看到那行E了吗，那个就是报错输出。他报错说，仓库没有找到Release文件。  
这样，会修的就知道怎么修了，不会修发给别人，他也能快速帮助你。他的问题是没有完整添加软件源，导致系统不知道跑哪里下载软件安装包。

### 怎么提供信息

有些时候，上网搜也搜不到解决方案，这时我们就需要求助于人了。对于初学者来说，这很正常。  
为了节省双方的时间，请各位在上网实在找不到解决方案的时候，再去求助他人。在询问问题的时候，请尽量提供详细的信息。  
比如，你的输入法没有拼音输入，你应该提供你系统的截图和设置选项。这比直接问“我的输入法没法输入中文”好多了。

### 不要过于依赖别人！

我知道对于初学者而言，有些问题搞不明白，得经常求助别人。这个很正常，我也是这么过来的。但是，解决问题后，你应该从中学到一些东西。如果你一直停留在出现问题-询问问题-解决问题的惯性中，你很难学的好。所以，不要过分依赖他人！要学会自己解决问题，逐渐学到更多。而且，人都是有七情六欲的，你一直问，会把人问烦的。

## 为什么我不推荐大家使用Linux当作日常系统

嘿嘿嘿，看完了是不是很迷糊，那就快跑！  
记住这些，一定要让那些冲动的人们不要踏进来！

### 专业软件太少

举两个例子：我高二的时候，有一会需要剪视频，使用Openshot，结果用起来没有Premiere方便不说，还经常崩溃，我被迫装回Windows，使用Premiere。然后是我刚买来新手机的时候，我刷机失败，需要救砖。但是救砖软件是Windows独占，在我用虚拟机救砖失败后，我被迫装回Windows来救砖。  
所以说，如果你有十分专业的需求，比如剪视频、重度办公、机床控制、3D游戏之类的话，Linux并不适合你。

### 社区风气极差

我最后为啥要给各位介绍如何优雅问问题/上网搜资料呢？因为Linux社区对小白很不友好。这里直接贴上《提问的智慧》的最后一段：

>如何更好地回答  
态度和善一点。问题带来的压力常使人显得无礼或愚蠢，其实并不是这样。  
对初犯者私下回复。 对那些坦诚犯错之人没有必要当众羞辱，一个真正的新手也许连怎么搜索或在哪找 FAQ 都不知道。  
如果你不确定，一定要说出来！ 一个听起来权威的错误回复比没有还要糟，别因为听起来象个专家好玩就给别人乱指路。要谦虚和诚实，给提问者与同行都树个好榜样。  
如果帮不了忙，别妨碍。 不要在具体步骤上开玩笑，那样也许会毁了用户的安装──有些可怜的呆瓜会把它当成真的指令。  
探索性的反问以引出更多的细节。 如果你做得好，提问者可以学到点东西──你也可以。试试将很差的问题转变成好问题，别忘了我们都曾是新手。  
尽管对那些懒虫报怨一声“读读该死的手册”（RTFM）是正当的，指出文档的位置（即使只是建议做个谷歌关键词搜索）会更好。  
如果你决意回答，给出好的答案。 当别人正在用错误的工具或方法时别建议笨拙的权宜之计，应推荐更好的工具，重新组织问题。  
帮助你的社区从中学习。当回复一个好问题时，问问自己 “如何修改相关文件或 FAQ 文档以免再次解答同样的问题？”，接着再向文档维护者发一份补丁。  
如果你是在研究一番后才做出的回答，展现你的技巧而不是直接端出结果。毕竟“授人以鱼，不如授人以渔”。

在我们的社区，这样的人很少，而且我去Bilibili上面看了，很多都是炫技/营销号:-P  
希望大佬们好好看看这里吧。

### 这是幽幽子使用的系统

这个无需多言，我们凡人使用了她用过的系统，岂不是要折寿？  
摘自zh.moegirl.org:

> 幽幽子平时使用Debian GNU/Linux，因为天冠上的标志与Debian GNU/Linux极为类似。 

以此类推，灵梦用的是Ubuntu，魔理沙使用的就是Arch Linux了吧233

## 推荐读物

[提问的智慧](https://hub.fastgit.xyz/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/main/README-zh_CN.md)  
[由书名号同志编写的资源搜索指南](https://lvris.com/p/resource-search/)  
[Arch Linux Wiki](https://wiki.archlinux.org/)  
[鸟哥的私房菜](https://www.vbird.org/)  

## 结尾

感谢大家阅读，希望这个文章能帮助大家适应Linux。我提到的很多东西，在Windows下也适用呢。
