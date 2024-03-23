+++ title = "Traintime PDA v1.1.0 版本发行概要" slug = "Traintime PDA v0.4.1 Release Note" description = "Traintime PDA 大回顾" date = "2024-03-23" image = "https://legacy.superbart.top/picture/xdyou/homepage.jpg" categories = [ "Technology" ] tags = [ "Flutter", "编程", ] +++

Traintime PDA v0.4.6 和 v1.0.0 差别只有桌面小部件；而 v1.1.0 和 v1.0.0 除了日程表的差别，就是为了新学期必将出现的流汗黄豆瞬间而做了一部分修改。

现这个程序终于搞完了，我可以搞点别的事情了。

本发行概要，将对本程序开发以来，遇到的所有技术要点，进行总结。所以，很多东西我将照抄我之前版本的发行概要。同时，我也将之前发行概要的小前言放在下面，可能为了语序，有些修改。

# 自发布后的版本更新概要

 - v0.1.0：第一个稳定版本
 - v0.2.0：空闲教室查看功能；移除西电目录，使用电话本代替；很多的 WebView 功能；应某个工作室请求，我写了个双创需求大厅；顺利上架 F-Droid
 - v0.3.0：iOS 版本添加吉祥物；应用内信息
 - v0.4.0：物理实验查看功能
 - v0.4.1：iCalendar 导出课表
 - v0.4.2：用户添加自定义课程功能
 - v0.4.3：滑块验证码适配
 - ~~v0.4.4：版本跳过，数字不吉利~~
 - v0.4.5：异步加载功能
 - v0.4.6：新的卡片设计
 - v1.0.0：桌面小部件
 - v1.1.0：桌面小部件完善；移除打卡功能；课程表升级为日程表

# 贡献者名单一览

现在基本写完了，正好是致谢贡献者的时候了。
以下名单包括任何在代码库留下痕迹的人，以及给过我设计稿的人。更加详细的请看代码仓库的[这个文件](https://github.com/BenderBlog/traintime_pda/blob/main/lib/page/setting/about_page/about_page.dart)。

| 功能                                                                                                                     | 贡献者                                                             |
| ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------ |
| 最佳&最久故障反馈者                                                                                                      | [BellssGit](https://github.com/BellssGit)                          |
| 主页，登录页，配色，iOS 小部件等                                                                                         | [BrackRat](https://github.com/BrackRat)                            |
| 滑块验证码位移量修复                                                                                                     | [chitao1234](https://github.com/chitao1234)                        |
| 辅助修复滑块问题                                                                                                         | [Dimole](https://github.com/Dimole)                                |
| 体育页面设计                                                                                                             | [EliteWars](https://space.bilibili.com/49892391/)                  |
| XDYou 图标设计                                                                                                           | [木生睡不着](https://node.kg.qq.com/personal?uid=669e9a84212d348f) |
| 卡片设计稿 [Re_X_Card.dart](https://github.com/BenderBlog/traintime_pda/blob/main/lib/page/public_widget/re_x_card.dart) | [Reverier-Xu](https://github.com/Reverier-Xu)                      |
| XDYou 开屏图，支持 iOS 开发                                                                                              | [Ray](https://ray.al)                                              |
| 首页时间轴卡片，异步加载                                                                                                 | [stalomeow](https://github.com/stalomeow)                          |
| XDU Planet & 设置页面                                                                                                    | [xeonds](https://mxts.jiujiuer.xyz)                                |
| Android 小部件，原生和 Flutter 端拼接代码                                                                                | [Xiue233](https://github.com/Xiue233)                              |
| 丁香电费修复                                                                                                             | [ZCWzy](https://github.com/ZCWzy)                                  |

欢迎各位有想法的参与开发～

# 版本号对应歌曲简介

主要在彩蛋里面，大家可以多去听听：

| 版本号 | 歌曲                                   | 作者          | 专辑 - 发行年份 - 发行商                    |
| ------ | -------------------------------------- | ------------- | ------------------------------------------- |
| 0.4.0  | Dreams Never End                       | New Order     | Movement - 1980 - Factory                   |
| 0.4.1  | Tequila Sunrise                        | The Eagles    | Desperado - 1973 - Asylum                   |
| 0.4.3  | Supertzar / Symptom of the Universe    | Black Sabbath | Sabotage - 1975 - Vertigo                   |
| 0.4.5  | Temple of the King / Catch the Rainbow | Rainbow       | Ritchie Blackmore's Rainbow - 1975 - Oyster |
| 0.4.6  | The Perfect Kiss                       | New Order     | Low-Life - 1985 - Factory                   |
| 1.0.0  | Ripples...                             | Genesis       | A Trick of the Tail - 1975 - Charisma       |

# 新的日程表简介

(来自 v0.1.0 发行概要，原先内容作废)

日程表集合了原先的课表，同时我加上了考试信息和物理实验信息。为了更加方便显示，只能引入午休和晚饭时间，并将一天结束时间定在21:25。另外，很久之前糊上去的数组也没了，改成了类似[flutter_calendar_view](https://pub.dev/packages/calendar_view)这样，按照连续时间来创建日程卡片。如果这个功能可以彻底和 PDA 解耦合，并将其用于其他的项目，这也许将是能够击败任何闭源课表的无敌存在(bushi)

这个实际上是我最开始写 PDA 的动机，很简单，一个页面显示所有跟我们学业相关的所有信息，不香吗？之前那个程序早期版本是彻底割裂的，后来也是仅仅在主页上搞了仨按钮，一个课表、一个考试、一个物理实验。其中后俩还是在明天/接下来有状况的状况下才有，总觉得有种割裂的美。

## 课程信息数据模型介绍

此处只介绍课程表相关的信息，文件在`lib/model/xidian_ids/classtable.dart`。关于考试信息，请看`lib/model/xidian_ids/exam.dart`；关于物理实验信息，请看`lib/model/xidian_ids/experiment.dart`。

### 课程信息

包括课程名称及序号，和班级序号。其中只有课程名称是必须填入的，课程序号和班级序号是可选项。

这主要是用来标识课程名称的，为接下来的时间安排部分铺路。

```dart
class ClassDetail {
  String name; // 课程名称
  String? code; // 课程序号
  String? number; // 班级序号

  ClassDetail({
    required this.name,
    this.code,
    this.number,
  });
}
```

### 时间安排

包括以下部分：

 - 课程索引：课程信息在课程信息数组中的位置。下面我将介绍课程信息数组。
 - 数据来源：在 PDA 中，有教务系统获取的课程，有用户添加的课程。这两类信息分别是保存在两个不同的课程信息数组的，这里用一个枚举类型`Source`变量来表示区别。
 - 上课周次：一个布尔变量的数组，长度一定程度上代表了该学期的长度。从第一周开始，该周有课是`true`，否则是`false`。
 - 星期几上课，第几节上课，第几节下课：分别使用一个`int`变量表示。请注意这里是将一天分成十节课来处理的，课程时间参见文件。星期几上课根据学校后端返回数据，从 1 开始，代表星期一。
 - 可选教师信息：这个时间安排应该由谁上课，由于涉及到调课改变老师的因素，故在这里储存。
 - 可选教室信息：上课地点信息。

这里解释上课周次中“一定程度”的意义。实际使用中，大多数数据确实能符合这个情况，但很显然，会有很多的例外。我有个活跃用户，他有个课叫金工实习，那个课程的上课周次数组元素比其他的多 5 个，导致最后课表有很多的空白页面。这里讲道理，很无语，但暂时没有好办法解决。

另外有一个引申变量：

 - 上课长度：下课时间减去上课时间的长度，用于渲染课程表。

另附数据来源里面提到的枚举类型变量。

```dart
enum Source {
  empty,
  school,
  user,
}
```

以及上课时间安排数组，交替上课开始时间和结束时间。比如第一个元素是第一节课的上课时间，第二个元素是第一节课的下课时间。

```dart
List<String> time = [
  "8:30",
  "9:15",
  "9:20",
  "10:05",
  "10:25",
  "11:10",
  "11:15",
  "12:00",
  "14:00",
  "14:45",
  "14:50",
  "15:35",
  "15:55",
  "16:40",
  "16:45",
  "17:30",
  "19:00",
  "19:45",
  "19:55",
  "20:35",
];

```

### 课程调整信息

最复杂的类型，由于涉及到很多学校后端的东西，所以进行了很多注释，用来表示与其的对应关系。同时，我对后端数据没有进行过多处理，大多数进一步分析都是在该类里面处理的。

包括以下元素：

 - 调课类型：包括调课，停课，补课。这个将以一个枚举类型`ChangeType`来展示。
 - 课程号，班级号，课程名：受影响课程的名称等信息。
 - 原来周次信息和新的周次信息：都是一个可空的布尔数组，从第一周开始，如果有影响则为`true`，否则为`false`。
 - 原先的老师和新的老师：为可空字符串，涉及到判断老师是否改变。
 - 原先的课次信息和新的课次信息：一个只有两个元素的`int`数组，表示开始上课时间和停止下课时间。如果没有提供，默认给`[-1,-1]`。
 - 原先的周次信息和新的周次信息：为可空的`int`变量，表示是否更改了上课时间。
 - 原先的位置信息和新的位置信息：为可空的字符串变量，表示是否更改了上课地点。

附带有这些方法：

 - 获取原先调整的周次和这次调整的周次信息：提取原来周次信息和新的周次信息数组中`true`的下标，方便后续处理。
 - 判断老师是否改了：从后端获取的前后老师信息对比，看出老师信息改了没。这里涉及到一定的正则表达式和学校后端返回的老师信息，所以本程序里面是以我们学校的返回格式编写的。学校返回的老师信息使用半角逗号和空格`, `分隔，老师信息包含姓名和工号，中间用反斜杠`/`分隔。所以我这里，先去掉空格，然后用逗号和反斜杠来分隔，最后去掉工号，排序保证顺序一致。由此对两个数组进行处理，论证处理后的数据是否一致来判断是否更改老师。
 - 显示给用户的原先老师和新老师信息：根据上面提供的信息，去掉数字和斜杠就好。
 - 一个转换 ChangeType 为字符串的玩意。

其中，调课类型枚组`ChangeType`如下：

```dart
enum ChangeType {
  change, // 调课
  stop, // 停课
  patch, // 补课
}
```

### 总体信息

在理想中，这将是传入课程表页面的东西。但在 PDA 中，由于添加删除用户自定义课程功能，导致必须引入程序的控制器，导致这个想法暂时没有实现:P

但是，思想还是保留了下来。

 - 学期长度：通过所有时间安排的上课周次数组中，最长的那个。
 - 开学日期和当前学期代码，用来生成每周的索引和对应课程等。
 - 两个课程信息数组。一个是教务系统发给的，一个是用户自行输入的。再次注意，里面有
 - 两个时间安排数组，再次注意，里面有标识课程信息的类型和课程信息索引。
 - 未排课数组，主要为了展示。
 - 课程改变信息数组，用来进行调课处理。

还有两个构造方法：

 - 通过时间安排元素来获取对应的课程信息。
 - 一个拷贝的构造函数，传入一个`ClassTableData`生成个一模一样的副本。

### 用于缓存的用户添加课程数据

这里只是为了方便保存数据而写的，保存了用户自行添加的课程信息和时间安排。

```dart
class UserDefinedClassData {
  List<ClassDetail> userDefinedDetail;
  List<TimeArrangement> timeArrangement;
}
```

## 日程表页面概览

这里介绍 PDA 代码中，用于渲染日程表的代码。代码在`/lib/page/classtable`下面，组成如下：

```
lib/page/classtable
├── arrangement_detail
│   ├── arrangement_detail.dart 引入显示信息的东西
│   ├── arrangement_detail_state.dart 保存需要显示信息的状态
│   ├── arrangement_list.dart 日程信息卡片清单
│   ├── course_detail_card.dart 显示课程信息的卡片
│   ├── custom_list_tile.dart 用于卡片的左图标右文字部件
│   ├── exam_detail_card.dart 显示考试信息的卡片
│   └── experiment_detail_card.dart 显示物理实验信息的卡片
├── class_add 添加课程页面
│   ├── class_add_window.dart 课程添加主页面
│   └── wheel_choser.dart 滚轮选择器，用于选择周次课程时间等
├── class_change 调课清单显示
│   └── class_change_list.dart
├── class_not_arranged 未安排时间课程清单
│   └── not_arranged_class_list.dart
├── class_page
│   ├── classtable_page.dart 课程表页面骨架页面和状态
│   └── empty_classtable_page.dart 空白课程表页面
├── class_table_view 日程表页面
│   ├── class_card.dart 日程表卡片
│   ├── class_organized_data.dart 用于规划日程信息的类
│   ├── class_table_view.dart 显示日程信息的页面
│   └── classtable_date_row.dart 显示日期和星期
├── classtable.dart 课程表页面使用入口
├── classtable_constant.dart 一些用于显示/渲染的常量
└── classtable_state.dart 课程表页面的状态
```

请在理解 `InheitedWidget` 和 `ChangeNotifier` 之后再来看。

## 一天的区分

和前身课程表不同，本表格由于添加了午休和晚饭时间，所以需要说明一天的划分状况：

* 早晨的 1-4 节课，每节课可以细分成 5 个小块来处理
* 午休按照 3 块来处理，表示 12:00 - 14:00
* 下午的 5-8 节课，每节课可以细分成 5 个小块来处理
* 晚饭时间按照 3 块来处理，表示 17:30 - 19:00
* 晚上的 9-11 节课，每节课可以细分成 5 个小块来处理

很显然，这每个小块所代表的时长是不一样的，这是一种设计上的妥协。在实际计算中，如果不是按照课程那种“比较规整”的角度来决定开始结束的，那就需要计算时间在格子上的相对位置了。

## 日程的计算与展示

首先，我们要知道`ClassOrgainzedData`是个啥。这是一个用来计算在一天之内，如何渲染卡片的类。它有六个属性：

1. 开始的位置和结束的位置，分别计为`start`和`stop`。
2. 要显示的日程名称`name`和位置`place`。
3. 卡片要显示的颜色`color`。
4. 这段日程之内，有多少同时进行的，计为`data`。这是个动态类型的List，设计中只允许`TimeArrangement`,`ExperimentData`,`Subject`(考试信息)。

具体如何计算开始和结束的位置，请结合“一天的区分”章节来看针对各个状况写的构造函数。

现在我们考虑一天之内，日程是如何展示的。以下的逻辑是在`/lib/page/classtable/class_table_view`里面，函数`List<Widget> classSubRow(int index)`过程的展现：

1. 如果传入的索引是 0，进行对最左边时间划分信息轴的渲染。否则进行下面的步骤。
2. 从课程表信息中的时间信息`timeArrangement`，考试信息`subject`，物理实验信息`experiments`中，获取对应周次日期的日程信息。
3. 按照开始时间排序。
4. 按照`_checkIsOverLapping`分析可能的重复情况，如果有重复，按照重复的所有日程中最早的开始和最晚的结束，定义新的日程信息，其中包括所有这段时间里面经历的信息。这里应该能保证，最终形成的日程不会有任何交集。
5. 根据生成的日程安排信息，使用`Positioned`组件，直接定位对应的位置，构建课程卡片。其中顶部和高度还要考虑到手机状态或平板状态，在`blockheight`中定义；左侧宽度由对应日期在一周中的位置确定。

最终，这些东西一起扔到一个`Stack`里面，加上最上面显示日期周次信息的`ClassTableDateRow`，完成对日程表的渲染。`ClassTableDateRow`代码在`classtable_date_row.dart`，有三处值得注意：

1. 需要计算那一周周一的日期。
2. 今天的颜色需要不一样。
3. 长宽比不同的时候，字体的颜色不同。

以上代码大幅度参考`flutter_calendar_view`插件。

## 显示详细信息

课程卡片点进去是会有弹窗，提示里面有啥详细信息的。这里涉及到`ClassOrgainzedData`里面那个数组了，它会传给`ClassCard`，然后当弹窗时候，传给`ArrangementDetail`。注意，`ArrangementDetail`会调用`ArrangementDetailState`，所以，还是得先懂`InheritedWidget`是啥:P

## 用户选择周次

称为`currentWeek`，首次加载的时候会默认从控制器里面获取计算好的周次信息，并进行判断。如果不在学期长度范围内，则按照情况渲染第一周/最后一周日程表。

当然，每次用户更改周次的时候，我们都需要刷新状态，所以我使用了 setter 来简化编程。

```dart
// Current showing week.
int _chosenWeek = 0;

// Change chosen week.
set chosenWeek(int chosenWeek) {
  _chosenWeek = chosenWeek;
  notifyListeners();
}

int get chosenWeek => _chosenWeek;
```

顺便在此说明，为了精简页面，日程表切换是用`TabBarView`实现的。切换的`TabBar`只会显示周次信息，不会再显示课表概览了。

## 输出课表到 iCalendar

先从课程表页面状态读取生成的 iCalendar 字符串，然后进行分享。

生成该文件内容的代码在称为`iCalendar`的 getter 里面，以下是一些不正经介绍。

Initally I want to output classtable schedule to the system calendar, but it isn’t good. So far, it outputs the class schedule one by one, from the first class of the first week to the last class of the last week. I have to let users agree serveral times to import all class schedules, so ummm…

So I use iCalendar, a standard to transfer schedules. It can transfer the name of your schedule, the time range (start time and end time), and lots of additional infos, including email, alert, personnel etc.

For the class schedules, aka classtable, we only care about the place, time, and the name. According to the [**CYBER GOD OF OUR SCHOOL**](https://blog.woooo.tech/posts/about_linux_desktop/), we do not need a "iCalender parser library", just treat it as a plain text file with mime-type `text/calendar`.

I will introduce a very simple iCalendar file here, and of course, it isn't cover all about iCalendar.

The whole iCalendar file is covered by `BEGIN:VCALENDAR` and `END:VCALENDAR`. While every schedule is covered by `BEGIN:VEVENT` and `END:VEVENT`. For each schedule, we can input the following attributes:

- `SUMMARY`: A quickview of the schedule. In our case, the class’s name and place.
- `DESCRIPTION`: Detail description of the schedule. We can put teacher info in here.
- `DTSTART` and `DTEND`: The start time and the end time of the schedule. Notice we need to follow the time pattern `yyyyMMddTHHmmss`, a description is at [here](https://icalendar.org/iCalendar-RFC-5545/3-3-5-date-time.html) and [here](https://pub.dev/documentation/intl/latest/intl/DateFormat-class.html).

Finally, a refrence of my code about output the iCalendar string.

```dart
String get iCalenderStr {
  String toReturn = "BEGIN:VCALENDAR\n";
  for (var i in timeArrangement) {
    String summary =
        "SUMMARY:${classDetail[i.index].name}@${i.classroom ?? "待定"}\n";
    String description =
        "DESCRIPTION:课程名称：${classDetail[i.index].name}; 上课地点：${i.classroom ?? "待定"}\n";
    for (int j = 0; j < i.weekList.length; ++j) {
      if (!i.weekList[j]) {
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

分享方式，在移动端使用了[`share_plus`](https://pub.dev/packages/share_plus), 这个库很简单。我要记一笔关于临时文件的东西。这个东西分享文件，是先在软件的临时目录中保存，然后分享，临时文件的清理就交给了系统。我是自己写了一套保存到临时文件，分享后立刻删除的机制。顺便，一定要设定`mime-type`为`text/calendar`，要不然存的就是纯文本文件。

在桌面端，则使用[file_picker](https://pub.dev/packages/file_picker)库，通过里面`saveFile`函数获取到要存储的位置，然后进行保存。

## 显示未安排课程和课程调整信息

这两个页面分别读取状态里面的未安排课程信息和课程调整信息，然后展示，没了()

## 添加课程页面

这个页面，在功能上只是对接控制器 -> PDA 自己的代码，但是我们还是需要搞明白咋传的数据。

我们需要以下数据：

- 课程名称，老师，地点
- 上课周次，时间（星期几上课，第几节课开始，第几节课结束）

前面的都好办，仨输入框控制器解决问题。后面时间的获取稍微有点麻烦，需要用到一个布尔数组和三个整型变量，代表周次和上课开始结束。上课周次是一系列的按钮，按啥改变数组对应的地方。选择周次和上课开始结束则涉及到`wheel_chooser`，一个根据纵向`PageView`，每页变化时候执行相应函数的组件。详情请阅读这些代码，非常直观。

当用户决定提交的时候，判断用户数据是否符合要求：

- 必须输入课程名称
- 上课时间不能大于下课时间

之后，将用户的输入转成课程信息和时间安排，执行控制器对应的代码，更新课程表页面。

# 横屏幕和竖屏幕

(来自 v0.1.0 发行概要)

我的程序做了一点平板的优化，我由此更加了解了 Flutter 的响应式开发。

## 如何在 Flutter 侦测横屏幕竖屏幕

Flutter 本身有很多的属性部件，比如`Theme`用来访问主题属性，`Navigator`访问路由栈之类。这里我使用的是`MediaQuery.of(context).size`，这是用来侦测当前页面长宽高状态的。实际上，上面我提到的很多高度检测啥的，都是用这个实现的。

而侦测屏幕位置，有两个思路：
 - 长宽比，长大于宽就是横着，否则就是竖着。
 - 之前我看到一篇文章说宽度 480 是个坎，小于算竖着。

我这里使用了后者的想法，前面的想法我就不写了：

```dart
bool isPhone(context) => MediaQuery.of(context).size.width < 480;
```

顺便说一句 LayoutBuilder， 是用来给部件加约束的组件，具体看官方指南吧。

## 我的 BothSideView

先给大家看看这玩意到底是个啥东西：

![](https://legacy.superbart.top/picture/xdyou/both.side.sheet.gif)

如你所见，在竖屏的时候，他是从底往上呼出的，跟 [BottomSheet](https://m3.material.io/components/bottom-sheets/guidelines) 一样；在横屏的时候，他是从右向左呼出的，和 [SideSheet](https://m3.material.io/components/side-sheets/overview) 一样。

Flutter 的 Material 框架本身没有实现 SideSheet ，而对于横屏来说，BottomSheet 是十分浪费屏幕，而且不太好看，从左面呼出是更合适的。得亏有很多的大佬，自行实现了 SideSheet 插件，我可以直接拿来使用他们的概念，但我想把这两个结合在一起。

而为啥要将这两个东西合在一起呢？这就涉及到实际使用中，我们是如何呼出 BottomSheet 了。

呼出 BottomSheet 和呼出 Dialog 一样，是使用了一个函数，在这里，叫 `showBottomSheet`。这玩意有个问题，他本质上是往路由栈里面压入一个 BottomSheet 页面栈，也就是说，无论横屏幕还是竖屏幕，他永远是 BottomSheet，而不会变化一点。我一开始用了 SideSheet，结果发现横屏开了 SideSheet，竖屏过来了还是 SideSheet，他们之间不会互相转化。

那我就缝合吧，SideSheet 好办，抄过来先辈的代码就好了，顺便我抄过来使用 `showGeneralDialog` 来显示弹窗了。但是 BottomSheet 本身并没有任何代码资料，我只能自己写了。我使用了 StatefulWidget 来保存 heightForVertical 变量，这是个高度变量，默认为页面高度的 80% 。然后我使用了一个 GestureDetector ，手势侦测器。这个侦测器在拖拽最上面的小横杠时候进行当前高度检测，然后更新高度。这里我将收起的高度定为页面高度的 40% 。

这里我说明一下 BottomSheet 和 SideSheet 的特点，他们都可以分成两个部分，上面的和下面的。下面的是传参传进来的部件，上面的就是属于部件的东西了。

最后再说一句，原来的 SideSheet 的最上面是使用 `AppBar` 实现的，但是 AppBar 会侦测手机的状态栏，最终导致在某些情况下，上面的高度过高。我被迫自行实现了这里，搞得很难看。

本组件代码在`/lib/page/public_widget/both_side_sheet.dart`，可能我考虑发个包。

## PageView 组件使用

还是跟组件状态玩命。

原先，我的首页是抄的 [Flutter 的 M3 实例](https://flutter.github.io/samples/material_3.html)。这样我就可以在横屏幕时候使用左侧的 [NavigationRail](https://m3.material.io/components/navigation-rail/overview)，竖屏幕的时候使用底部的 [NavigationBar](https://m3.material.io/components/navigation-bar/overview)。

那么，问题在哪？我原先写的组件，将横屏渲染和竖屏渲染函数给分开写了。结果就导致前几天我迁移首页四个卡片到 PageView 的时候，出现了横屏和竖屏切换时候，页面永远会刷新到第一页。一开始我看了好久的 StatefulWidget 的状态周期，我也没明白。最后我发现，我这是两个组件，每次刷新的时候都会重新绘制这两个组件。解决方法就是，将这两个组件合二为一，在一个组件里面渲染，使用 `Visibility` 组件按需隐藏。

# Webview Cookie 相关

(来自 v0.2.0 发行概要)

想在 Flutter 使用 Webview ，你可以使用两个插件：[webview_flutter](https://pub.dev/packages/webview_flutter) 和 [flutter_inappwebview](https://pub.dev/packages/flutter_inappwebview)。前者是官方开发，功能基础；后者是第三方开发，功能强大。我为了保证简洁，使用的是前者。

关于插件，网上很多资料都是很老的，我参考了这位的文章：[在 Flutter 中使用 webview_flutter 4.0](https://juejin.cn/post/7196698315835260984)，其中最有用的是第三篇，讲怎么用 Cookie 的。我的程序是这样写的：
1. WebView 页面中，接受要前往的网站和获取 Cookie 的网站。
2. 在 initState 状态下，初始化 Webview 的 CookieManager 和 Controller。WebView 的控制器可以控制加载，页面前进和回去。
3. 在 didChangedDepencies 状态下，根据获取 Cookie 的网站，从 Dio 的 CookieJar 中获取 Cookie。然后控制器请求对应网站。

具体代码在[这里](https://github.com/BenderBlog/traintime_pda/blob/main/lib/page/homepage/toolbox/webview.dart)。

最后，这个玩意貌似在 iOS 平台下有 bug，Cookie 死活加不进去，我已经彻底摆烂了:P

# 上架 F-Droid 平台

(来自 v0.2.0 发行概要)

F-Droid 有两个好：
 - 开源的东西多，就是好
 - 目前我程序在安卓平台唯一可以“自动更新”的方式

Flutter 程序上架，除了官方的，可以参考这位的[上传指南](https://friesi23.github.io/flutter/android/fdroid/appstore/2023/06/08/submitting-your-flutter-app-to-fdroid.html)。我想补充两点————可重复构建，分开架构构建：

F-Droid 的可重复构建，对我而言，最主要的就是使分发都带上我的签名。这就需要保证构建元数据需要你签名的 sha256 摘要，和一个可供对照的构建（在我这里就是我在 Github Action 上面的构建）。

分开架构构建，就是按照手机架构（arm64，arm32，x86）来构建分发包。这个东西，貌似每个架构的版本构建号还不一样。当时写构建元数据的时候，写到弃疗。他们 F-Droid 的审核人好好，帮我写了T_T

我的上架过程可以看看[这个链接](https://gitlab.com/fdroid/fdroiddata/-/merge_requests/13537)，合并请求后四天，真正上架。你们可以从这里[点进链接下载](https://f-droid.org/zh_Hans/packages/io.github.benderblog.traintime_pda/)。

另外说为啥来这里上架，我这软件确实是自由软件。还有，国内上架需要这个那个的，感觉好麻烦，而且已经有电表了，再上架一个感觉也吸引不了多少。

(现在更好了，备案号我是有，但我是个人名义，很多平台不认2333)

# 双创需求大厅相关

(来自 v0.2.0 发行概要)

这个东西，主要是使用了 Dart 3 的最新语言功能：Records。详情[看这个文章](https://juejin.cn/post/7233067863500849209).

我没记错，go 好像能一次性返回两个值。一开始我感觉很神奇，然后相似的东西就降临到 Flutter 了。说回来，如果没有这个东西，我会考虑 Pair / List，大不了写个 class 。

双创需求大厅本质上跟找工作网站差不多，都得有个 Popup 来选择职位状况。这个东西的服务器筛选工作，是需要两个东西：一个 String 传大致分类，一个字符串数组传输 tags。我选择这俩东西的部件是写在外面的，需要返回数据的话，我直接写 `(String, List<String>)` 就可以了。读取的这些数据的话，可以通过 `$1` 或 `$2` 来读取。

不过这玩意现在只有五个数据，以后会不会变多呢？也许我能通过这个，说一波我程序和xxx合作？

# 关于物理实验，乱码处理和 Dio 转换器

(来自 v0.4.0 发行概要，其中 Iconv 内容是本版本新添加的)

我们学校目前的物理实验服务器使用的是 2005 年的 ASP 技术，重点在 2005 年。实际上技术差点也没啥，但是有两点属实离谱：

1. 所有的信息都是用 GB2312 编码的。
2. 传回的 Cookie 有中文字符的字段。

其中第二点是最离谱的。

对于 Dart 底层的默认 UTF-16 String 来说，这俩点属实头疼。

## 乱码处理

乱码实际上很常见，常知道的锟斤拷梗就跟这个相关。毕竟汉字跟英文一样，在电脑底层都是需要用二进制编码来表示的。简体中文汉字有两个主要编码：

1. 国标码：一个用于编码汉字和一些日韩字符的国家标准，主要有 GB2312，GBK，GB18030 三个标准，呈现继承与发展（向下兼容）的特性。请查看[这个链接](https://zhuanlan.zhihu.com/p/453675608)来搞清国标码(GBK)相关。Windows 默认就是这个编码。国标码是定长编码，基本使用两个字节(16 位二进制位)来表示一个汉字。
2. UTF 编码：国际上有个统一码联盟，他们负责给全世界所有的字符编码，称为 Unicode。很早他们就支持了中日韩三个语言字符的编码（由于文字特性，中日韩字符在他们的体系中，在一个分区）。Unicode 只是规定了字符对应的二进制表示，但实际使用，位数过长而且浪费很多，所以实际使用只能继续缩短，使用更短的变长编码，称为 UTF。UTF 分成很多版本，一般代表了最短编码位数是多少。Linux / Mac + 互联网数据一般都是用这个编码。详情可以查看[这个链接](https://zhuanlan.zhihu.com/p/427488961)。

说到变长编码知识，计算机组成会讲汇编命令是如何编码的，那里会讲的。

很明显，如果用 UTF 编码解析国标码，绝对会解析出不正常的数据。大巧不巧，Dart 语言的 String 本质上是一个 UTF-16 编码的序列。于是问题就产生了。

国标码是定长编码，而 UTF 是变长编码，很显然是基本没法兼容的。不兼容还好，在我的实践中，用 UTF 编码先编码回二进制信息，然后用国标码解码信息，大概率是无法得到正确的数据。

所以我目前程序中，需要让网络库不能用 Dart 的 String 来解码我的数据，我需要一个支持国标码的解码库。

## Dart / Flutter 的 GBK 解码库

这个实际上有两种：

1. 流行方案：使用 UTF 和 GBK 的码表一一对应，方便转换。这个方式对平台很灵活，缺点需要让我程序增大 500k 左右，而且这种方式在执行时候也会有些慢。
2. 调用系统的解码接口来解码信息，我使用的是这个方案。但是缺点也很明显，如果没有对目标系统适配，解码就很难办。

最终我使用的是这个库：[charset_converter](https://pub.dev/packages/charset_converter)。它目前能 Windows，Android，iOS 三个系统的转码，而且使用很方便。他支持很多编码，但我主要用国标码。

## 关于 Dio 的转换器

Dio 的网络请求使用的是过滤器流水线模式：

> HTTP 请求 -> 若干拦截器 -> 转换器 -> Dart 底层实现或系统网络实现
> 响应的二进制码 -> 转换器 -> 若干拦截器 -> HTTP 响应

拦截器一般处理 Cookie，判断响应码之类。目前 Dio 的拦截器不支持异步方法。

转换器 Transformer 是一个二进制码和 HTTP 请求响应结构互相转化的桥梁。默认的 Transformer 是解码后用来对 body 进行判断的。由于我上面提到，不能用 UTF 先编码再解码，所以我定制了一个 Transformer，称为 `ExperimentDioTransformer`。在一些基本对 Body 的二进制解析后，直接用 GBK 解码库来返回数据。学校物理实验服务器都是返回的网页，所以这么写没啥问题。

(现在新版本的拦截器可以用异步办法了，但我不想改了)

## 关于 Cookie 有中文字符

可以看看[我在 Dio 开发仓库提出的问题](https://github.com/cfug/dio/issues/1959)。

Cookie 的官方规范，是仅允许一部分 ASCII 码作为合法字符的，Dart 核心库的 Cookie 实现严格遵照这个规范。但是令我哭笑不得的是，咱学校物理实验服务器传回的 Cookie 包含中文字符，就是这个用户的名字。加上 GBK 导致的编码，最后的结果自然就是报错，扔出“错误编码异常”。

人官方严格按照标准，无可厚非。我为了这个玩意折腾了很长时间，直到最后，有个人告诉我，那个 Cookie 给服务器传任何值都可以，我无语了......

## 关于 Iconv 转码的两三事

这是搞 Windows 和 Linux 构建的副产品，针对`charset_converter`搞的新扩展。Pull-Request 链接[在此](https://github.com/pr0gramista/charset_converter/pull/38)。

接下来说明`IConv`库，Unix 下面用来转码的库。实现很多，这里我使用的是 GTK 里面自带的 Iconv 库，因为 Flutter for Linux 使用的是 GTK，这样做能减少依赖。顺便，给 Flutter 写 Linux 原生代码有个注意点，好歹要对`GLib`这玩意有点了解。我之前速通过`Vala`，所以不算过于陌生。

我主要利用了这些函数：

### 生成实例函数

```C
GIConv
g_iconv_open (
  const gchar* to_codeset,
  const gchar* from_codeset
)
```

(`gchar` 是 `GLib` 里面，对于 `char` 的别称)

传入的是两个字符串，要转成的字符编码`to_codeset`和源文本的字符编码`from_charset`。返回的是一个`GIConv`转换器，或者，如果“打开转换器失败”(多数情况就是没找到编码)，就返回`(GIConv)-1 `。

这个函数也可用于侦测编码集是否支持，代码如下：

```C
gboolean isAvaliable = (iconv_cd = g_iconv_open("UTF-8", fl_value_to_string(charsetName))) == (GIConv)-1;
```

### 编码解码函数

```C
gchar*
g_convert_with_iconv (
  const gchar* str,
  gssize len,
  GIConv converter,
  gsize* bytes_read,
  gsize* bytes_written,
  GError** error
)
```

这个是主要的解码函数，强制需要输入要解码的字符串`str`，字符串长度`len`，和转换器`converter`；不强制需要的是存储读取字符的地址`bytes_read`，存储写入字符数量的地址`bytes_written`，以及一个存储报错信息的地址`error`。

关于这里，想多说几句。`charset_converter`插件提供的接口本身没有提供待转换字符串的长度，这对其他原生平台没有问题，但对 Iconv 来说，有点要命。Iconv 接受的是一个用`\0`结束的，正宗 C 语言字符串，但是 Flutter 传过来的时候没有。而 C 语言没有侦测内存大小的函数，我的 C 语言基本也快忘了，所以情况很离谱。如果我不知道字符串长度，而直接转换，基本上不是末尾多了一个字符，就是少了几个，甚至直接报错。我直接改这个插件 Dart 端的函数形参很不现实，最主要的是这插件不是我写的。所以最后，我只能在 Dart 端先给字符串加个末尾的`\0`，然后传原生。

### 关闭实例函数

```C
gint
g_iconv_close (
  GIConv converter
)
```

传入要关闭的转换器`converter`，-1 失败，0 成功。

# 应用内信息的分发机制

(来自 v0.4.0 发行概要)

借鉴了[这个项目](https://github.com/xeonds/xdu-planet)。接下来，根据我的“服务器”和借鉴项目的 Github Action 配置文件，我给大家做一个大致的部署过程讲解。

## 借鉴项目的 Action

Go 版本的 XDU Planet，本质上就是 RSS 处理转 json，然后用 gin 开服务器端口。这个项目使用 Github Action 来每小时更新，然后更新成一个 json 文件，最后搞到 Github Page。

这个项目有三个分支：主代码，配置文件，部署分支。发布流程大致如下：

1. Action 环境初始化，获取代码（Checkout）。
2. 对代码进行构建，对于这个项目，就是构建 go 代码和 vue 代码。
3. 使用 go 生成的可执行文件，生成 json 文件。
4. 上传生成的网页和 json 到部署分支，然后在部署分支的基础上部署 Github Action。

## 我的“通知服务器”

可以看看[这个链接](https://github.com/BenderBlog/traintime_pda_backend)。核心技术就是用 [Miller](https://github.com/johnkerl/miller) 来将 csv 转换成 json，然后用 Github Action 推到 Page 服务。同样的，这个项目有两个分支：

1. main 分支：存储 csv 文件和 Github Action 配置文件。
2. depoly 分支：存储需要通过 Github Page 发布的 json 文件。

发布流程和上面的差不多：

1. Action 环境初始化，获取代码（Checkout）。
2. 将 csv 转换为 json 文件。
4. 上传 json 到部署分支，然后在 depoly 分支的基础上部署 Github Action。

# 关于一点点 iOS 开屏娘的事情

(来自 v0.4.0 发行概要，其中 Iconv 内容是本版本新添加的)

这个玩意主要用到了 XCode 的界面设计工具。长这样：

![](https://legacy.superbart.top/picture/xdyou/XDYou_XCode_LaunchImage.png)

Apple Store 上架需要程序有个开屏图，我于是找个人画个漫画。画家顺便画个手绘板的图标，风格对应了。

这个玩意我当时搞了接近一个下午才搞成，大部分时间在摸索这玩意到底咋用，小部分时间在看各个手机屏幕大小情况下的排版状况。最终我摸索出这样的排版：

1. 上面人脸下面图标，在一个中轴线上。
2. 人脸大小写死，因为我不知道如何动态调整图片大小:P 图标比例写死 1:1。
3. 人脸中心在 Y 轴中心上面(减去) 80px 处，图标在 Y 轴下面(加上) 200 像素处。

# 某滑块验证码

以下简要介绍本次学校升级的滑块验证码工作原理，默认传的都是用的一个 cookie 。

1. 此处略去如何加密密码，算法没变。

2. 文字验证码不再需要，学校前端不再判断需要输入验证码与否。`https://ids.xidian.edu.cn/authserver/checkNeedCaptcha.htl` 不在需要。

3. 滑块验证码需要先传递这个：
```dart
await dio.get(
      "https://ids.xidian.edu.cn/authserver/common/openSliderCaptcha.htl",
      queryParameters: {'_': DateTime.now().millisecondsSinceEpoch.toString()},
);
```

1. 然后进入滑验证码页面，传输数据：
```dart
/// 往这个接口发送 get 请求，然后返回的数据是个 json ，有三个比较重要
/// 背景图是 bigImage，滑块图是 smallImage，tagWidth 我这里暂时没用到.png
/// 滑块图的高度和 bigImage 大致是相同的。
provider = dio.get(
      "https://ids.xidian.edu.cn/authserver/common/openSliderCaptcha.htl",
      queryParameters: {'_': DateTime.now().millisecondsSinceEpoch.toString()},
      options: Options(headers: {"Cookie": widget.cookie}),
).then((value) async {
      var provider = SliderCaptchaClientProvider(
            value.data["bigImage"],
            value.data["smallImage"],
            double.parse(value.data["tagWidth"].toString()),
      );
      await provider.init();
      return provider;
});
```

1. 得到数据后，我程序的前端渲染一个滑块验证码窗口让用户滑，然后得到 offset。然后和写死的宽度给服务器传过去鉴权。成功的话退出滑块页面，进行登录，否则刷新滑块。如果用户未完成滑块就退出滑块页面会报错“动态验证码错误”。
```dart
bool result = await dio.post(
    "https://ids.xidian.edu.cn/authserver/common/verifySliderCaptcha.htl",
    data: "canvasLength=${(snapshot.data!.puzzleWidth)}&"
              "moveLength=${(_sliderValue * snapshot.data!.puzzleWidth).toInt()}",
    options: Options(headers: {
                            "Cookie": widget.cookie,
                            HttpHeaders.contentTypeHeader:
                                "application/x-www-form-urlencoded;charset=utf-8",
                            HttpHeaders.accessControlAllowOriginHeader:
                                "https://ids.xidian.edu.cn",
                        })
).then((value) => return value.data["errorCode"] == 1);
/// 涉及页面内异步，使用 mounted 判断是否异步完毕
if (mounted) {
    result ? Navigator.of(context).pop() : setState(() { updateProvider(); });
}
```

结束介绍，这破滑块验证码我还以为有啥高级的，结果就这？修这玩意最累的就是写这个滑块页，然后把我干感冒了。

顺便学校搞个文字验证码怕是比这个强吧，这个玩意随便一个 opencv 搞个亮度边缘检测估计就能破，都没必要让用户滑。文字验证码好歹还得文字识别，要真搞爬虫的话，也比这个难吧。不过 PDA 不打算实现自动化滑块验证码，因为这样总觉得有点黑产的感觉，或者说，“破坏计算机信息系统罪”？

顺便附上咱学校的滑块验证码用的[前端库源代码地址](https://gitee.com/LongbowEnterprise/SliderCaptcha)，这基本没改啊:-\ 我对这个玩意真是无语了，修这玩意把我干感冒更是让我想骂街。所以我拉出了 Black Sabbath 的 Sabotage 专辑两首歌，[Symptom Of The Universe](https://music.163.com/#/song?id=16837102) 和  [Supertzar](https://music.163.com/#/song?id=16837105) 分别当作本版本 iOS 和 Android 的发行代号。

这两首歌的背景实际上也是种不满。Black Sabbath 乐队当时被乐队经理告上法庭，他们疲于为了法律问题奔命，根本没心情写歌。于是他们搞出来他们这一辈子最重的专辑。

# 教务移动端抓包记录

一开始，为了提高 PDA 程序的稳定性，我决定在某些功能方面弃用 ehall ，这玩意半夜经常挂掉。所以，我打算使用我在学校小程序里面看到的教务新前端，兴许这玩意能稳定点，而且确实，在编程方面能简单些。但是后来我发现，这玩意挂掉的概率更高，所以我又改了回去，目前只有空闲教室使用了这个。

这个不适用于 v1.0.0 之后的版本。

## 如何登录

在统一认证服务认证用户后，需要有个跳转地址，这个地址根据我的抓包，应该是这个：

```
https://xxcapp.xidian.edu.cn/a_xidian/api/cas-login/indexredirect=https%3A%2F%2Fxxcapp.xidian.edu.cn%2Fuc%2Fapi%2Foauth%2Findex%3Fappid%3D200190304164516885%26redirect%3Dhttps%253A%252F%252Fehall.xidian.edu.cn%252Fjwmobile%252Fauth%252Findex%26state%3DSTATE%26qrcode%3D1&from=wap
```

他很长，很显然用到了其他的二级鉴权服务，但是我们不需要关心，在最开始跟着他 302 的 Location 跳转就好了。为啥是“在最开始”，因为跳转的 Location 里面有很重要的鉴权标志。

接下来的内容涉及到 Authorization 头，建议各位先看个[这玩意的介绍](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Authorization)。我的程序中，有个双创招队伍展示的功能，那个的后端也是用的这玩意鉴权，而且显然比这玩意好写。

我上面提到了很多的 302 跳转，其中最重要的跳转是在 xxcapp 之后 get 的第一个 ehall ，那里面的 Location 充满了 token，那个 token 就是我们之后要用到的 Authorization token。

之后用功能的时候，需要在 Header 里面使用 Authorization，传过去这个 token 就行。

在使用一个服务的时候，根据抓包看出的规律，我们需要给 https://ehall.xidian.edu.cn/jwmobile/biz/home/updateServiceUsage POST 一个 json 编码的数据，形式是 {"key": 你要搞的服务}。

其中你要搞的服务有这些：

```json
{
	"data": [
		{
			"service_key": "XK.MTXKSH",
			"servicename": "免听选课审核"
		},
		{
			"service_key": "XJ.XJYDSQ",
			"servicename": "学籍异动申请"
		},
		{
			"service_key": "XJ.XYYJCX",
			"servicename": "学业预警查询"
		},
		{
			"service_key": "XJ.XYWCCX",
			"servicename": "学业完成查询"
		},
		{
			"service_key": "KW.HKSQ",
			"servicename": "缓考申请"
		},
		{
			"service_key": "KW.HKSH",
			"servicename": "缓考审核"
		},
		{
			"service_key": "KW.KSAPCX",
			"servicename": "考试安排查询"
		},
		{
			"service_key": "KW.JKAPCX",
			"servicename": "监考安排查询"
		},
		{
			"service_key": "KW.CJCX",
			"servicename": "成绩查询"
		},
		{
			"service_key": "PK.TDBKSQ",
			"servicename": "调停补课申请"
		},
		{
			"service_key": "PK.TDBKSH",
			"servicename": "调停补课审核"
		},
		{
			"service_key": "PK.JSJYSQ",
			"servicename": "教室借用申请"
		},
		{
			"service_key": "PK.JSJYSH",
			"servicename": "教室借用审核"
		},
		{
			"service_key": "PK.KXJSCX",
			"servicename": "空闲教室查询"
		},
		{
			"service_key": "QT.SHKSBM",
			"servicename": "社会考试报名"
		},
		{
			"service_key": "XQBG",
			"servicename": "学期报告"
		},
		{
			"service_key": "JX.HD.TP",
			"servicename": "投票"
		},
		{
			"service_key": "JX.HD.WD",
			"servicename": "问答"
		},
		{
			"service_key": "JX.HD.SP",
			"servicename": "视频"
		},
		{
			"service_key": "JX.HD.ZL",
			"servicename": "资料"
		},
		{
			"service_key": "JX.HD.PF",
			"servicename": "评分"
		},
		{
			"service_key": "JX.HD.XR",
			"servicename": "选人"
		},
		{
			"service_key": "JX.HD.QD",
			"servicename": "抢答"
		},
		{
			"service_key": "SH.WDSQ",
			"servicename": "我的申请"
		},
		{
			"service_key": "SH.DWSH",
			"servicename": "待我审核"
		},
		{
			"service_key": "XK.MTXKBL",
			"servicename": "免听选课办理"
		},
		{
			"service_key": "KW.CJRDSQ",
			"servicename": "成绩认定申请"
		},
		{
			"service_key": "PK.QXKBCX",
			"servicename": "全校课表查询"
		},
		{
			"service_key": "PJ.XSPJ",
			"servicename": "学生评教"
		},
		{
			"service_key": "XJ.DLFLSQ",
			"servicename": "大类分流申请"
		},
		{
			"service_key": "XJ.ZZYSQ",
			"servicename": "转专业申请"
		},
		{
			"service_key": "TC.TCCJLR",
			"servicename": "体测成绩录入"
		},
		{
			"service_key": "PJ.DDPJ",
			"servicename": "督导评教"
		},
		{
			"service_key": "XJ.FXSQ",
			"servicename": "辅修申请"
		},
		{
			"service_key": "PJ.PJBG",
			"servicename": "评教报告"
		},
		{
			"service_key": "PJ.WDPJ",
			"servicename": "过程性评教"
		},
		{
			"service_key": "TC.TCCJCX",
			"servicename": "体测成绩查询"
		},
		{
			"service_key": "KW.CJRDSH",
			"servicename": "成绩认定审核"
		},
		{
			"service_key": "PJ.GRPJZHTJ",
			"servicename": "个人评教综合统计"
		}
	]
}
```
在你 POST 之后，如果鉴权成功，则会返回一个空的 body 。没看错，是空的 body，为了这玩意我折腾了大约三分钟来写判断代码。如果返回了一个 json body，然后其中的 code 是 401，那就需要重新获取 token 了。基本上，重新获取就是重新走遍登录流程()

最后需要说明的是，我不知道这个步骤是否是必须的，也许不走这个步骤也能获取到数据。但是这个步骤目前是我所知能够最好判断 token 刷新的途径了。应该他们的前端也是这么实现的。

## 成绩获取

上集说过如何先“进入”一个程序吧，接下来我们需要获取数据并处理数据。不得不说，这个接口传递的方式很简单，获取的数据也是足够的干净。

首先是如何获取，很简单，给这个地方 POST 个数据就好：

```dart
var getData = await authorizationDio.post(
      "https://ehall.xidian.edu.cn/jwmobile/biz/v410/score/termScore",
      data: {"termCode": "*"},
).then((value) => value.data);
```

其中注意到 data，是传递的学期信息，格式类似 `2023-2024-1` ，代表2023年到2024年第一学期。星号代表所有学期的所有信息。

回复的信息中，如果 code 不是 200 ，不用想，肯定有问题。所以接下来就给大家看看具体信息长啥样

根据这个信息，我们就可以处理简单的展示了，接下来我们需要进一步处理数据。

我的程序是可以计算均分的，这里需要引入对成绩构成的定义：

1. 百分制度，0-100 给分数
2. 五级赋分制，给优秀(95)、良好(85)、中等(75)、及格(65)、不及格(0)
3. 三级赋分制，给优秀(95)、通过(75)、不通过(0)
4. 二级赋分制，给通过(75)、不通过(0)，注意不计算 GPA (八成给数据也是 0 学分)
5. 免修计算为 85 分

据说还有两级赋分制，不过那些课是不计算 GPA 的，所以到时候再看。

那么，我们要计算均分，需要知道成绩信息里面的成绩字符串。这个成绩字符串可以是纯数字，也可以是上面提到的字符串。在计算均分的时候，我们需要转换这些字符串，这不难。而且我发现，GPA 和分数是紧密相关的。

## 考试信息获取

这是我觉得这个接口最好的一点：数据结构十分清晰。接下来我将体现这一点。

如何进入考试应用请看上集提到的东西，代号是：`KW.KSAPCX`。

进入课程信息后，我们需要获取学期信息。这个学期信息我觉得是非常优秀的，因为返回数据中有一个布尔值，表示是否为当前学期。我的代码中有一段代码，如果接口无法访问，就使用我程序缓存的目前学期的代码。

```dart
    /// Choose the first period...
    developer.log("Seek for the semesters.", name: "Jiaowu getExam");
    String semester = await authorizationDio
        .get("https://ehall.xidian.edu.cn/jwmobile/biz/v410/examTask/termList")
        .then((value) {
      for (var i in value.data["data"]) {
        if (i["currentFlag"] == true) return i["termCode"];
      }
      return preference.getString(preference.Preference.currentSemester);
    });
```

获取到当前学期信息后，我们就可以搞考试信息了。这个接口和一站式的接口，最大的区别是没有老师的信息。我觉得这个不是大问题，因为每次考场都会写上老师的名字。而且我们考试前都去上课记老师名字吧233
考试信息结构体如代码中 Subject 里面指出的那样。

```dart
    /// If failed, it is more likely that no exam has arranged.
    developer.log("My exam arrangemet $semester", name: "Jiaowu getExam");
    List<Subject> examData = await authorizationDio
        .get(
      "https://ehall.xidian.edu.cn/jwmobile/biz/v410/examTask/listStuExamPlan"
      "?termCode=$semester",
    )
        .then((value) {
      if (value.data["code"] != 200) {
        throw GetExamFailedException(value.data["msg"]);
      }

      var data = value.data["data"];
      return List<Subject>.generate(
        data.length,
        (index) => Subject(
          subject: data[index]["courseName"],
          type: data[index]["batchName"].toString().contains("期末考试")
              ? "期末考试"
              : data[index]["batchName"].toString().contains("期中考试")
                  ? "期中考试"
                  : data[index]["batchName"],
          time: data[index]["timeNote"],
          place: data[index]["classroomName"],
          seat: int.parse(data[index]["seatNo"]),
        ),
      );
    });
```

最后获取没有考试安排的科目，只有课程名称和课程 id。

```dart
    List<ToBeArranged> toBeArrangedData = await authorizationDio
        .get(
      "https://ehall.xidian.edu.cn/jwmobile/biz/v410/examTask/listStuExamUnPlan"
      "?termCode=$semester",
    )
        .then((value) {
      if (value.data["code"] != 200) {
        throw GetExamFailedException(value.data["msg"]);
      }

      var data = value.data["data"];
      return List<ToBeArranged>.generate(
        data.length,
        (index) => ToBeArranged(
          subject: data[index]["courseName"],
          id: data[index]["courseNo"],
        ),
      );
    });

```

获取数据之后，我们需要处理数据，先看接口返回的数据结构：

```json
{
	"0": {
		"batchId": "726db6ccda7f42f8b532a6b365e2a8ca",
		"batchName": "2021-2022学年第一学期期末考试",
		"courseNo": "CS263004X",
		"courseName": "数据结构",
		"examStart": "2022-03-17 00:00:00",
		"timeStart": "13:00",
		"timeEnd": "15:00",
		"campusNo": "S",
		"campusName": null,
		"buildingNo": "B",
		"buildingName": null,
		"classroomNo": "B-312",
		"classroomName": "B-312",
		"seatNo": "45",
		"timeNote": "2022-03-17 13:00-15:00(星期四)",
		"examTaskStatus": "1"
	}
}
```

我利用的是 timeNote 来计算考试时间，毕竟~~代码迁移量小~~能用 RegExp 搞明白的事情，就不搞这么多东西。
我只需要侦测开启时间就好，我们用这个正则表达式`[0-9]{4}-[0-9]{2}-[0-9]{2} [0-9]{2}:[0-9]{2}`就能提取开始时间，进而进行对比。

空闲教室请各位自行看代码吧，当时写到这里的时候接到了很多用户的崩溃报告，我直接弃疗，谁用这倒霉东西谁后悔我跟你们说。

# 安卓小部件和苹果小部件

首先，安卓小部件我是求别人写的，苹果部件是我写的。(Swift UI 是真的好写)

## 苹果的 groupid 机制

关于数据读取，安卓的好办，直接读就行了。但是苹果的就难办了。

简单来说，苹果的小部件是独立于主程序的，而苹果的沙盒机制导致了程序之间的数据是无法共享的。各位从 App Store 上面下载的程序，本质上是一个套件，里面包含了主程序和小部件程序。虽然小部件程序不能直接读主程序的数据，但是苹果大发慈悲地提供了一种名字叫`groupid`的机制，来使一个套件下面的数据能够共享。对于这个东西的介绍，可以阅读[这个文章](https://www.jianshu.com/p/a2d9ef58bad5)。

我们需要共享的数据，主要是课表数据。所以我们需要把课表数据从主程序的区域复制一份到公共区域，而这里又涉及到调用原生端代码的问题，因为那些存储到公共区域的代码都是原生代码。这里我们需要提到[Flutter 的 Pigeon 插件](https://pub.dev/packages/pigeon)，通过这个插件，我们可以比较简单地访问到原生代码。具体使用这里不会说明，我主要介绍我在原生开放了啥接口。

这个接口需要获取一个`appid`组名称，一个文件名`fileName`，文件里面的内容`data`（必须为纯文本）。同时需要实现一个函数`saveToGroupId`来实现保存到公共空间的功能。接下来展示的就是我给插件编写的模板接口文件，插件会根据这个文件生成用于访问该函数的类。顺便一提，这个文件不能放进`/lib`里面。

```dart
// Copyright 2024 BenderBlog Rodriguez and contributors.
// SPDX-License-Identifier: MPL-2.0

import 'package:pigeon/pigeon.dart';

@ConfigurePigeon(PigeonOptions(
  dartOut: 'lib/bridge/save_to_groupid.g.dart',
  dartOptions: DartOptions(),
  swiftOut: 'ios/Runner/SaveToGroupID.g.swift',
  swiftOptions: SwiftOptions(),
  copyrightHeader: "pigeon_bridge/copyright_header.txt",
))
class FileToGroupID {
  FileToGroupID({
    required this.appid,
    required this.fileName,
    required this.data,
  });
  String appid;
  String fileName;
  String data;
}

@HostApi()
abstract class SaveToGroupIdSwiftApi {
  String getHostLanguage();

  @async
  bool saveToGroupId(FileToGroupID data);
}

abstract class SaveToGroupIdFlutterApi {
  bool saveToGroupId(FileToGroupID data);
}
```
接下来就需要在 iOS 端实现这个实现。

```swift
func saveToGroupId(data: FileToGroupID, completion: @escaping (Result<Bool, Error>) -> Void) {
    let fileManager = FileManager()
    do {
        let fileURL = FileManager.default.containerURL(forSecurityApplicationGroupIdentifier: data.appid)
        if fileURL == nil {
            throw AppIdFailedError()
        }
        print("\(String(describing: fileURL?.absoluteString))")
        if fileManager.fileExists(atPath: fileURL!.absoluteString) {
           try! fileManager.removeItem(at: fileURL!)
        }
        try Data(data.data.utf8).write(
            to: fileURL!.appendingPathComponent(data.fileName),
            options: [.atomic,]
        )
    } catch is AppIdFailedError {
        completion(.failure(FlutterError(
            code: "AppIdFailedError",
            message: "Can't get the folder with appid",
            details: "You should check whether your app group id spells wrong."
        )))
    } catch {
        completion(.failure(FlutterError(
            code: "WriteFailedError",
            message: "\(error)",
            details: error.localizedDescription
        )))
    }
    print("Write complete!")
    completion(.success(true))
}
```

这样，在主程序数据更新的时候，通过执行这个函数，我们可以将更新后的数据同步到代码公共区域，然后，使用[home_widget](https://pub.dev/packages/home_widget)插件，来让小部件强制刷新。

## 数据处理

这里处理方式双端大差不差：

1. 读取课程表文件，考试信息文件，物理实验文件。
2. 从这些文件中提取出当日/明日信息，并转化为用于显示的日程类。
3. 日程类渲染展示。

此外，苹果部件在渲染前还包含一步，也就是创造小部件时间轴，根据时间删除过去的日程。这个功能安卓没实现，因为安卓后台刷新更加玄学。或者这么说，所有平台的后台刷新都是玄学级别，不如苹果小部件的时间轴可以控制。

## 小部件刷新

安卓的是靠一个内置的时钟，每十八分钟更新。不过这玩意玄学能不能更新，只能看天。

苹果是按照时间轴机制刷新的，每天半夜根据文件内容，生成当天的时间轴，包括今天和明天的内容。关于时间轴请查看[苹果的官方介绍](https://developer.apple.com/documentation/widgetkit/keeping-a-widget-up-to-date)。我的处理方法是，先获取数据，整理后根据各个日程的下课时间来决定时间轴上面的坐标，对应坐标是当时还在进行的课程和尚未开始的课程，最后进行刷新。

在此之前，我打算使用一个后台刷新插件，名字叫 [background_fetch](https://pub.dev/packages/background_fetch)，通过这个东西打算在后台让程序运行，进而在主程序主页的日程组件更新日程的时候，顺带更新小部件的数据。但是后台刷新在任何平台都是玄学，加上开学后有个叫跑步的玩意彻底让体育小插件没有存在的意义，顺便带走了后台刷新的意义，于是就这样了()

# 程序主页日程处理

把课程信息，考试信息和物理实验信息统合到一个日程表（在竞争品那边叫课表）是我最开始写 PDA 时候的愿望。在 v1.1.0 版本中，通过对其他日历程序的东西，终于实现了。

## 统一的日程格式

首先，我们需要做到一个统一的日程格式，通过这个来方便显示。这个类称为`HomeArrangement`，主要包括日程名称，老师，地点，开始时间和结束时间。

```dart
class HomeArrangement {
  static const format = 'yyyy-MM-dd HH:mm:ss';

  String name;
  String teacher;
  String place;
  @JsonKey(name: 'start_time')
  String startTimeStr;
  @JsonKey(name: 'end_time')
  String endTimeStr;

  DateTime get startTime => DateTime.parse(startTimeStr);
  DateTime get endTime => DateTime.parse(endTimeStr);
}
```

本程序中的数据来源，都需要按照这个格式，根据天数，返回对应天数的日程。接下来，我们按照我们数据来源，来看到主页上的东西。

## 数据来源一览

这些数据来源，为了保证编程容易和单例模式，使用了`GetX`里面的`GetController`控制器。使用`Get.put()`来在程序里面任意时候调用**关于这个数据的唯一实例**。

| 名称         | 控制器名称           | 获取日程接口                                                      |
| ------------ | -------------------- | ----------------------------------------------------------------- |
| 课程表       | ClassTableController | `List<HomeArrangement> getArrangementOfDay(DateTime timeToQuery)` |
| 考试信息     | ExamController       | `List<HomeArrangement> getExamOfDate(DateTime now)`               |
| 物理实验信息 | ExperimentController | `List<HomeArrangement> getExperimentOfDay(DateTime now)`          |

## 桌面数据刷新进程

这里的代码涉及“后台登陆刷新功能”，是 PR 来的功能。

在用户刷新主页信息的时候，或者从别处回到本程序主页的时候，日程数据就会刷新。这里简单涉及到了`AppLifecycleListener`，侦测生命周期的东西，详情可以看[这个链接](https://zhuanlan.zhihu.com/p/651402152)。在本程序中，我们需要侦测`resumed`状态，也就是前台运行状态。我们通过重写`didChangeAppLifecycleState`来侦测。

```dart
@override
void didChangeAppLifecycleState(AppLifecycleState state) {
	super.didChangeAppLifecycleState(state);

    if (state == AppLifecycleState.resumed) {
      updateCurrentData();
    }
}
```

# Windows 和 Linux 构建测试

这个想法来自于社区，我本身是没有考虑的。这就导致很多代码可能需要小改，因为我只考虑了手机和平板。所以，目前我只是发行了测试版，希望随着时间能够有所提升。

目前遇到的问题有：

- 通知桌面端做不到
- Linux 转码问题(已经解决)
- Windows 日历输出有问题(已经解决)

无论如何，构建都是相对直接的：安装好环境，直接构建。构建后的文件直接打包，按照绿色软件处理。

# 一些乱七八糟的玩意

## 关于开源的想法

我对软件，按照开源和开发者，这么看：

> 个人开发的开源软件或半开源软件 > 集体开发的开源软件 > 个人开发的闭源软件 > 集体开发的半开源软件 > 集体开发的闭源软件

其中，半开源软件请参考 [FDroid 的负面特征定义](https://f-droid.org/zh_Hans/docs/Anti-Features/)。显然我的软件属于半开源软件，我这个软件实质上模拟了你在浏览器中，对学校后端的访问。

实际上软件的开源与否，并不重要，重要的是软件本身能不能很好用，而按照我的经验，软件的好用也可以这么排序，尤其是手机端应用()

所以，我虽然经常说开源很重要，但这个实际上是因为我认为个人开发者的产品更好而导致的。而开源软件放前面，是因为代码开放让人用着更舒服，可能我长期用 Linux 留下来的某种遗留症状。而且我某种意义上，真的不喜欢封闭的东西，虽然我发现大家都喜欢。

而为啥我要将这个软件按照 MPL 授权，是因为我的软件有很多可以复用的东西，比如上面我大幅度提到的课程表和那个 BothSide 。这些复用的东西我将来是打算做成程序内的 package，如果按照 GPL ，不利于传播。而我目前程序状态，如果使用 MIT 之类的，那可能会有很多的魔改版，然后闭源了。MPL 是按照文件强制开源的，就目前状态所言，假如你只是用了我的课程表代码文件，那么，你只需要开源课程表代码文件+你对这个代码的修改，就好了。

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

本来我是不想现在就上架 App Store，但是电表突然上架了。虽然目前功能少，但着实打了一惊，我也顾不上我软件的不成熟，也上架了。看来大家还是很认可我的软件，所以感觉可以。我也很感谢很多帮我的人，无论是画吉祥物的，还是帮我发传单的，给我 UI 设计提出建议的。

之前我好像说过学校“揭榜”的事情，这玩意确实有点用，就是在面试时候问项目背景的时候，至少能扯到学校:-P

但是到现在都没消息，还来个验证码，我********

## 某日本玩意相关

日本人能好好说话吗，最近看真寻酱动画片。里面的台词我听着很耳熟啊，超市真就发音苏坡马特(supermarket)，微笑也发音和斯迈尔里(smiley)神似，真离谱。

(此处狗头保命，顺便说明我不是男娘，为啥最近男娘这么流行)

## 移除体育打卡功能

我还是那句话，强迫人的东西，就算是好事，也是能给败坏了。但是我也不建议花钱刷，因为这种技术的玩意花钱我觉得真不合适，就跟刷机包要花钱买一样。不过我进一步想，技术开放的前提是很多人都能有能力，有时间来实践，而且至少有一点感恩心啥的。很显然不是所有人都这样，感恩心不用说，全是反例子。要不然当初为啥搞 LSPosed 那几大位心累如此呢？而前两个：有能力有时间，更重要的是有时间。就现在课程压力大+竞争压力大，我估计能有时间折腾的，不是满绩点和各种比赛都能玩的开，就是光搞兴趣爱好而把学业荒废的？

无论如何，现在这个功能被体育课程信息窗口查看。也许将来可以日程表信息可以包括这个？我暂时没想好。

