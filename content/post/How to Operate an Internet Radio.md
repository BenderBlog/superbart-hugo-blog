+++
title = "如何在网上开自己的电台？"
slug = "Reading Reviews"
description = "怀旧时间到～"
date = "2022-10-19"
image = "https://legacy.superbart.xyz/picture/How%20to%20Operate%20an%20Internet%20Radio/Bart%20On%20Radio%203x13.jpg"
categories = [
    "Technology"
]
tags = [
    "网络广播",
    "Icecast",
    "OBS",
    "Mixxx"
]
+++

实际上这个文章我老早就想写了，不过我中间基本上忘了这档子事。

先说明一下，这个电台是纯音乐电台，没有画面。要搞画面的话，建议了解 `nginx-module-rtmp`。

开一个电台，需要两个部分：电台推流软件，电台服务器，以及收听软件。就像传统的电台一样，得有个录音室，发射塔，然后是收音机。啊哈，是不是回到了各位童年，听着中国之声呢(不是)。

## 电台服务器～Icecast

实际上我接触过两个开电台的软件，一个是上面提到的 `nginx-module-rtmp`。不过这玩意更像是视频服务器，我就不想说了。

[Icecast](https://www.icecast.org) 是一个音乐电台服务器，也就是说，它接受电台推流软件传来的数据，经过处理(包装)，然后向收听软件推送数据。这里我就不说啥 `m3u8`，`ogg`是啥了，毕竟我也不知道，而且又不影响咱用，对不对啊:-)

既然 Icecast 是一个服务器软件，那么~~它就得运行在服务器上~~实际上是个电脑就能运行，不过最好还是个服务器吧，比如说你在网上买到的阿里云服务器之类。不过如果你只是想在你的家里搞个自嗨广播，那电脑直接运行也好。前提是你能处理好路由器端口映射和电脑的防火墙，那就不是我的事情了2333

以下我用 Linux 系统举例子，更特殊的说，是 Debian 11 。其他的发行版，应该能举一反三吧.......

使用这个命令安装 icecast

```bash
# apt install icecast2
```

安装完了，就得配置，看一下我这个配置文件片段吧，你的配置文件应该在 `/etc/icecast2/icecast.xml` 下面。

```xml
<icecast>
    <!-- 这俩选项主要是说明电台的地址(location)和管理员是谁，
         感觉就是为了展示用的 -->
    <location>SUPERBART'S LAND</location>
    <admin>icemaster@localhost</admin>

    <!-- 尤其对于小白用户，以下内容十分重要:
         最好一开始*只*修改密码，然后重启 Icecast 。
         想要详细配置说明的话，请查阅文档。
         文档也在这里提供：http://icecast.org/docs/
         (原来配置文件的一段)
    -->

    <!-- 请各位不要直接使用这个配置文件，看明白了修改自己电脑里的配置文件。-->

    <limits>
        <!-- 这段都是设置服务器的限制的，比如最多几个人听，最多开几个频道之类。
              大多数选项我也不太懂，尽量别改吧。
        -->
        <clients>200</clients>
        <sources>5</sources>
        <queue-size>25600000</queue-size>
        <client-timeout>30</client-timeout>
        <header-timeout>15</header-timeout>
        <source-timeout>10</source-timeout>
        <burst-on-connect>1</burst-on-connect>
    </limits>

    <!-- 这里修改各种密码，是重点捏 -->
    <authentication>
        <!-- 推流密码，推流账户名 source -->
        <source-password>这里输入你想搞的密码</source-password>
        <!-- 中继密码，中继账户名 source -->
        <relay-password>这里输入你想搞的密码</relay-password>

        <!-- 网页管理页面界面的账户和密码 -->
        <admin-user>admin</admin-user>
        <admin-password>这里输入你想搞的密码</admin-password>
    </authentication>

    <!-- 设置该软件监听哪个端口，一般无需改动 -->
    <listen-socket>
        <port>8000</port>
    </listen-socket>

    <!-- 这里我省略了好多选项，不要直接使用这个配置文件！ -->
</icecast>
```

当你修改好自己的配置文件，使用这个命令启动 Icecast 软件。

```bash
$ sudo service icecast2 start
$ sudo systemctl enable icecast2 # 如果你想让这玩意开机自启动的话
```

现在你打开你的服务器网站的 8000 端口的话，你应该能看到这个。我这里是开电台了，所以有东西，应该是啥都没有才对。

![](https://legacy.superbart.xyz/picture/How%20to%20Operate%20an%20Internet%20Radio/Icecast.png)

## 电台推流软件三例

在 Icecast 网站上，他们[贴了一堆软件](https://icecast.org/apps/)。我这里写三个我用过的。更多的话，可以看看[Arch Linux Wiki 的这篇文章](https://wiki.archlinux.org/title/Icecast)。

### OBS 还能推流广播？

你没看错，OBS 也能推流到 Icecast 服务器上，不过我觉得没有那么方便了。毕竟他只是个视频直播软件啊......

首先说点闲话，Icecast 能处理视频流，就是你们直播到 Bilibili 的那个。详情请看[这个链接](https://epir.at/2018/03/08/obs-icecast-streaming/)。不过这玩意不是官方支持，所以我不会过多描述，但是我以下的配置文件是根据这玩意改的。

打开 OBS 的设置界面，调到 输出 -> 录像 ，类型选择 "自定义输出 FFMpeg" 上面。然后咱这么修改：

1. FFMpeg 输出类型改成"输出到 URL"，下面的 URL 改成 `icecast://source:上面设置的推流密码@服务器ip或者域名:8000/stream`

2. 容器格式选择 opus(音频)，下面的混流器设置填上 `content_type=application/ogg`

3. 下面的音频比特率填个合适的，比如 192kbps 之类，想起了下载 MP3 年代了吗？

![OBS 设置一个例子](https://legacy.superbart.xyz/picture/How%20to%20Operate%20an%20Internet%20Radio/OBS-REFRENCE.png)

然后点击 "开始录制"，vola，你现在开始广播了！当然，画面是传不过去了，不过调整一下声音配置，你的声音开始传播了。

### Mixxx 感觉更适合些

Mixxx 是一个 DJ 软件，他能混音各种各样的音乐，也能按照顺序播放音乐......好吧，我对这个软件没有那么了解，只是知道这个东西可以用来广播:-P

首先，你最好有个歌库啥的，也就是说，你的电脑得有一堆歌曲文件。这玩意下载也没那么难吧，随便开个网易云，腾讯啥的，一堆可以下载。把他们放在一起，然后在软件设置里面规定好歌库位置。等待然后在音轨选项里面全选之，右键选择"放到自动DJ"。打开自动DJ界面，点击启用自动Dj按钮，好了，广播台现在能循环你的歌单了。歌单还能随机播放哦。

![Mixxx 界面概览](https://legacy.superbart.xyz/picture/How%20to%20Operate%20an%20Internet%20Radio/Mixxx-Main.png)

如果你想增加个麦克风的话，你可以在设置里面添加之。这个东西还能添加应用程序作为输入源呢，不过需要搞啥回环声音设备之类，我觉得很不好用，应该有更好的解决方案吧。

![设置输入设备](https://legacy.superbart.xyz/picture/How%20to%20Operate%20an%20Internet%20Radio/Mixxx-Microphone.png)

最后，就是规定你的广播地址了。这个看截图应该更明白吧......

![设置广播例子](https://legacy.superbart.xyz/picture/How%20to%20Operate%20an%20Internet%20Radio/Mixxx-Broadcast.png)

好了，开启你的自动DJ功能，合适时候开下麦克风，Let's on air!

### FFMpeg 极简主义

没错，ffmpeg 也能推流 icecast 捏。不过一般都是用来转播别的广播台233，图个测试用途。看看我用来推流中国之声到服务器上面的命令。

```bash
ffmpeg -re -i "https://ngcdn001.cnr.cn/live/zgzs/index.m3u8" \    # 音频源头
       -f opus -content_type application/ogg \    # 推流的格式和发送过去的 Content_Type
       -ice_description "转播中国之声" -ice_name "CNR News Transfer" \ # 该推流的描述和名称(不填写也可以)
       "icecast://source:上面设置的推流密码@服务器ip或者域名:8000/cnr-news-transfer" # 推流目的地址 
```

具体想知道咋用的话，看看[这个链接](https://ffmpeg.org/ffmpeg-protocols.html#Icecast)。

最后说一句，本人十分不建议推流视频到 Icecast 服务器上，挺难用的。( Icecast 官方说支持 opus 和 webm，我知道最后那个是视频格式 )

## 让听众听见你的声音

实际上这块很水了，点到为止得了......

一个途径是，让你的听众打开你的推流地址，应该可以直接播放，如果浏览器支持的话......

或者说，你给他们推流链接，让他们拿啥播放器打开......

## 最后

电台开起来了，放啥东西合适呢？实际上我也不知道，感觉听众不喜欢听新闻......

毕竟电台已经成为了某种怀旧的东西了，不过想想我在寒假那时候搭的电台，还是很能缓解一下某种孤独感的。我是那种倾向于向公众大喊"来看看我啊"的内向疯子:-P
