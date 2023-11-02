+++
title = "Traintime PDA v0.4.1 发行简记"
slug = "Traintime PDA v0.4.1 Release Note"
description = "Traintime PDA 0.4.1 的介绍和技术相关"
date = "2023-11-02"
image = "https://legacy.superbart.xyz/picture/xdyou/homepage.jpg"
categories = [
    "Technology"
]
tags = [
    "Flutter",
    "编程",
]
+++

# Traintime PDA 0.4.1 发行简记

先写个输出 iCalendar 功能吧，小部件我先慢慢写......

说是发行简记，实际上我要写很多的技术相关细节。

可以加入我程序的 [Testflight](https://testflight.apple.com/join/pLKe5B4q) 来尝鲜。

## 新功能和相关

支持 iCalendar 输出课程表信息和物理实验信息，[解释使用视频在此](https://www.bilibili.com/video/BV1yC4y1n7Q8)。

iOS 用户建议配合这个[快捷方式](https://www.icloud.com/shortcuts/6f951baebc534991868cf63958189030)使用。

加上了课程调整信息的处理。

## 关于技术相关

### 围绕介绍 iCalendar 的英语写作练习

Initally I want to output classtable schedule to the system calendar, but it isn't good. So far, it outputs the class schedule one by one, from the first class of the first week to the last class of the last week. I have to let users agree serveral times to import all class schedules, so ummm...

So I use iCalendar, a standard to transfer schedules. It can transfer the name of your schedule, the time range (start time and end time), and lots of additional infos, including email, alert, personnel etc. 

For the class schedules, aka classtable, we only care about the place, time, and the name. According to the [**CYPER GOD OF OUR SCHOOL**](https://blog.woooo.tech/posts/about_linux_desktop/), we do not need a 'iCalender parser library', just treat it as a plain text file with mime-type `text/calendar`.

I will introduce a very simple iCalendar file here, and that's not all about it.

The whole iCalendar file is covered by `BEGIN:VCALENDAR` and `END:VCALENDAR`. While every schedule is covered by `BEGIN:VEVENT` and `END:VEVENT`. For each schedule, we can input the following attributes:

 - `SUMMARY`: A quickview of the schedule. In our case, the class's name and place.
 - `DESCRIPTION`: Detail description of the schedule. We can put teacher info in here.
 - `DTSTART` and `DTEND`: The start time and the end time of the schedule. Notice we need to follow the time pattern `yyyyMMddTHHmmss`, a description is at [here](https://icalendar.org/iCalendar-RFC-5545/3-3-5-date-time.html) and [here](https://pub.dev/documentation/intl/latest/intl/DateFormat-class.html).


Finally, a refrence of my code about output the iCalendar string.

```dart
/// lib/page/classtable/classtable_state.dart line 47-72
/// Generate icalendar file string. 
String get iCalenderStr {
  String toReturn = "BEGIN:VCALENDAR\n";
    for (var i in timeArrangement) {
      String summary = "SUMMARY:${classDetail[i.index].name}@${i.classroom ?? "待定"}\n";
      String description = "DESCRIPTION:课程名称：${classDetail[i.index].name}; 上课地点：${i.classroom ?? "待定"}\n";
      for (int j = 0; j < i.weekList.length; ++j) {
        if (i.weekList[j] == '0') {
          continue;
        }
        Jiffy day =
            Jiffy.parseFromDateTime(startDay).add(weeks: j, days: i.day - 1);
        String vevent = "BEGIN:VEVENT\n$summary";
        List<String> startTime = time[(i.start - 1) * 2].split(":");
        List<String> stopTime = time[(i.stop - 1) * 2 + 1].split(":");
        vevent +=
            "DTSTART:${day.add(hours: int.parse(startTime[0]), minutes: int.parse(startTime[1])).format(pattern: 'yyyyMMddTHHmmss')}\n";
        vevent +=
            "DTEND:${day.add(hours: int.parse(stopTime[0]), minutes: int.parse(stopTime[1])).format(pattern: 'yyyyMMddTHHmmss')}\n";
        toReturn += "$vevent${description}END:VEVENT\n";
    }
  }
  return "${toReturn}END:VCALENDAR";
}
```

### 关于将得到的 iCalendar 字符串输出成文件

实际上没啥好说的，因为太玄学了。

我一开始使用了 `file_picker` 插件，打算让用户先选个文件夹，然后将文件保存了。结果我测试，全都崩溃了。根据反馈，是在插件的原生端获取到用户选择的目录后，返回给 Flutter 端的时候崩溃的，真够无语的。

![堆栈信息](https://legacy.superbart.xyz/picture/xdyou/file_picker_crash_1.png)

![具体到库代码](https://legacy.superbart.xyz/picture/xdyou/file_picker_crash_2.png)

正好我的开发时候大量依赖用户的网络交互记录反馈，这是依靠 `Alice` 插件实现的。我去读了 Alice 的代码，发现他使用了很多 `share_plus` 库，然后我的保存功能就使用了这个。

这个库很简单，我要记一笔关于临时文件的东西。这个东西分享文件，是先在软件的临时目录中保存，然后分享，临时文件的清理就交给了系统。我是自己写了一套保存到临时文件，分享后立刻删除的机制。顺便，一定要使用我之前提到的 mime-type，要不然存的就是纯文本文件。

关于 `share_plus` 库，请看这个[链接](https://pub.dev/packages/share_plus)。

### 课程冲突信息处理

有调课，停课，补课三种。调课是麻烦点，需要寻找到课程信息，然后找到所有跟此相关的时间信息，然后该删除的删除，该添加的添加。

不过咱学校的调课信息属实离谱，居然能有调整信息里的老师和对应课程信息里老师信息不一致的状况，离谱啊。最后我把老师信息提取到时间信息去了……

## 其他相关

日本人能好好说话吗，最近看真寻酱动画片。里面的台词我听着很耳熟啊，超市真就发音苏坡马特(supermarket)，微笑也发音和斯迈尔里(smiley)神似，真离谱。

(此处狗头保命，顺便说明我不是男娘，为啥最近男娘这么流行)

## Tequila Sunrise by Eagles

[MV 在此](https://www.bilibili.com/video/BV1Bz411e7MC)

```
It's another tequila sunrise
Starin' slowly 'cross the sky
Said goodbye

He was just a hired hand
Workin' on the dreams he planned to try
The days go by

Every night when the sun goes down
Just another lonely boy in town
And she's out runnin' 'round

She wasn't just another woman
And I couldn't keep from comin' on
It's been so long

Whoa, and it's a hollow feelin'
When it comes down to dealin' friends
It never ends

Take another shot of courage
Wonder why the right words never come
You just get numb

It's another tequila sunrise
This old world still looks the same
Another frame
```

Farewell my good time.
