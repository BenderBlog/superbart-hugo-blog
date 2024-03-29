+++
title = "Traintime PDA v0.1.0 发行简记"
slug = "Traintime PDA v0.1.0 Release Note"
description = "Traintime PDA v0.1.0 的介绍和技术相关"
date = "2023-07-29"
image = "https://legacy.superbart.top/picture/xdyou/homepage.jpg"
categories = [
    "Technology"
]
tags = [
    "Flutter",
    "编程",
]
+++

终于，经过一年多的断断续续的编写，Traintime PDA v0.1.0 发布了。虽然功能还算较少，但可以算是稳定版了。
Traintime PDA 是西电同志们的又一个个人信息查看应用，很明显，是电表的模仿产品。

v0.1.0 可以算是这个软件第一个稳定版本，我自然需要花上一小段篇幅来跟大家介绍这个软件。

说是发行简记，实际上我要写很多的技术相关细节。

## 功能介绍

1. 根据Timetable重写的 Flutter 课程表，这个课程表我尽力模仿这个插件，解决课程冲突，还能添加一张背景图片，能挂着你喜欢的 idol 之类()
![课程表页面](https://legacy.superbart.top/picture/xdyou/classtable.jpg)
2. 体育查询，打卡记录和体测成绩。
![体育查询页面](https://legacy.superbart.top/picture/xdyou/sport.jpg)
3. 成绩查询，包括可以自行选择科目计算均分。计算均分功能看来同学们十分喜欢使用，但我是大摆子(逃)
![成绩计算](https://legacy.superbart.top/picture/xdyou/score.jpg)
4. 自行选择学期的考试安排查询，自行选择学期功能是疫情的后遗症。
![考试查询](https://legacy.superbart.top/picture/xdyou/exam.jpg)
5. 电量查询和欠费查询，这个功能只是在首页上的卡片。
6. 校园卡流水查询和(如果有的话)校园卡余额查询。    
   (显示余额功能我考虑到手机支付十分广泛，首页就不显示了)
  ![流水查询](https://legacy.superbart.top/picture/xdyou/school.card.record.jpg) 
7. 图书馆信息查询，个人借书状况和学校书库状况。    
   (扫码借书，扫码转借功能担心风险，尚未支持)
  ![图书馆信息](https://legacy.superbart.top/picture/xdyou/library.png)
8. 西电目录，曾经在疫情封校期间运行的学校综合楼目录 + 食堂目录。
  ![西电目录](https://legacy.superbart.top/picture/xdyou/xddir.jpg)
9. XDU Planet：查看同学的博客，富含先辈的恩情（学习资料），另该功能代行转发学校教务处通知。
  ![XDU Planet](https://legacy.superbart.top/picture/xdyou/xduplanet.jpg)

## 关于相比电表的优势

我的程序打不过电表，这是肯定的。但我可以说出几点我的软件优势。

 - 我的程序使用 Flutter 开发，天生能适应 iOS 和 Android 两个移动端，使用范围肯定更广。我已经为 iOS 和 Android 都编译了目标端，在双端的运行效率都很流畅。
 - 我的程序代码完全开源，而且结构清晰明了。我给我的程序编写了[架构图介绍](https://legacy.superbart.top/writing/XDYou%20SAD.html)。这么做，可以保证别人可以阅读我的代码，然后修改代码，诞生他们学校的学生信息查看应用。而且我确信，这个是最能在开发者方面，保障用户隐私的最好方式。
 - 我的程序一定程度上适配了横屏，平板使用体验应该更好。看看上面图书馆的图片就能看出来了。
 - 我的程序很不正经。首先，开发者很不正经，而且保守的不得了；其次，程序里充满了彩蛋，甚至，我的字很好看(不是)。

## 关于接下来的任务

1. 空闲教室查看功能，这个我感觉使用量应该不高（也许是因为我是个大摆子）
2. 物理实验查询，我目前不做实验了，所以可能得找人帮忙了()
3. 校园网流量查询，目前学校校园网免费，啥时候要收费我赶紧写一个
4. 很多的 WebView 功能，比如报修啥的，我需要进一步研究
5. iOS 和 Android 小部件，我需要进一步研究，而且感觉影响不大
6. 扫码借书，扫码转借。这个我担心会对学校库存有所影响，而且难以测试，所以暂时不写

## 关于技术相关

这些东西是进一步介绍我程序里面的技术，很多在我看来不是最优解，欢迎大家指正。

我之前写了两篇：

 - 关于我们学校的系统后端，只有一站式服务中心
 - 关于我程序的架构

可能以后版本的发行简记不会这么详细了吧。

### 课程表

这里我尽量用 MVVC 模式介绍。

课程表写在了一个 StatefulWidget 里，方便维持一整个页面的状态，这个就是 View 。课程表的业务代码已经全部剥离到 classtable_controller.dart 里面，这个就是 Controller。

关于如何将 Controller 里数据传输到部件方面，也就是 ViewModel 方面，我使用的 GetX 框架，用了他两个状态管理方式，我再啰嗦一句吧：

 - `.obs + Obx()`将数据和状态绑定，部件观察数据更新而进行更新，这个是单向的状态传递;
 - `Controller.update() + 控制器注入或绑定到部件`，这个方式可以让部件发起控制器更新，是双向的。

课程表使用的是后一种，使用 `Get.put()` 方式，将控制器注入到课程表部件里面。

最后，是关于 Model 方面，这个是 Repo 里的东西，这里省去。

#### 数据模型介绍

这里我先介绍数据模型，也就是我将学校数据处理后的结果。文件在 lib/model/xidian_ids/classtable.dart 。

提前说明，有关于 json 序列化模板代码可以忽略。

##### 课程信息

包括课程名称及序号，教师名称，和班级序号。这里有很多可选选项，只能说学校就这么搞的（）
涉及到渲染时候判断课程信息相同，我重载了 hashCode 和 == 运算符。

```dart
@JsonSerializable(explicitToJson: true)
class ClassDetail {
  String name; // 名称
  String? teacher; // 老师
  String? code; // 课程序号
  String? number; // 班级序号

  /// 略去初始化代码

  @override
  int get hashCode => name.hashCode;

  @override
  bool operator ==(Object other) =>
      other is ClassDetail &&
      other.runtimeType == runtimeType &&
      name == other.name;
}
```


##### 时间安排

包括以下部分：

 - 课程索引，也就是上面课程信息在课程信息数组中的位置。下面我将介绍课程信息数组。
 - 上课周次，这里我继承了学校处理这个信息的方式。学校返回的是 0 和 1 组成的数组，0 代表这周没课程，1 代表这周有课。
 - 星期几上课，第几节上课，第几节下课。请注意这里是将一天分成十节课来处理的，课程时间参见文件。
 - 一个可选的教室信息。

另外有一个引申变量：

 - 上课长度就是下课减去上课。
 
```dart
@JsonSerializable(explicitToJson: true)
class TimeArrangement {
  int index; // 课程索引
  // 返回的是 0 和 1 组成的数组，0 代表这周没课程，1 代表这周有课
  @JsonKey(name: 'week_list')
  String weekList; // 上课周次
  int day; // 星期几上课
  int start; // 上课开始
  int stop; // 上课结束
  @JsonKey(includeIfNull: false)
  String? classroom; // 上课教室

  int get step => stop - start; // 上课长度

  /// 略去初始化代码
}
```

##### 总体信息

不仅包括上面提到的东西，还包括：

 - 学期长度：通过所有时间安排的上课周次数组中，最长的那个。
 - 开学日期和当前学期代码。
 
```dart
@JsonSerializable(explicitToJson: true)
class ClassTableData {
  int semesterLength;
  String semesterCode;
  String termStartDay;
  List<ClassDetail> classDetail;
  List<ClassDetail> notArranged;
  List<TimeArrangement> timeArrangement;
  /// 略去初始化代码
```


#### 控制器文件

控制器里包括了：

 - 课程数据，默认是空的。
 - 预先渲染好的课程表数据。
 - 当前是全学期第几周。
 - 当前课程信息。

##### 日期相关计算

首先，我的课程表要处理课次偏移信息，所以在获取学校的开学日期后，还得加减相应的周次，虽然可以不搞的()

计算利用到开学日期，一个公式就可以解决：

```dart
currentWeek = (Jiffy.now().dayOfYear -
              Jiffy.parseFromDateTime(startDay).dayOfYear) ~/
          7;
```

其中 Jiffy 是一个计算时间的库，这里我利用了他计算当前是全年第几天。

##### 预先渲染好的课程表数据

这里的数据将会在控制器初始化时候生成，在获取到 Repo ，或称 Model ，提供的课程信息后进行合成。

我这里直接使用了四维度数组，你们可以认为是稀疏矩阵。虽然这不是最优解，但是他还算容易访问；虽然复杂度很高，但是由于数据量很小，对性能影响不大。

四维度数组是这样的表示：

周次-星期-第几节课-这节课重叠了几节课

```dart
// A list as an index of the classtable items.
late List<List<List<List<int>>>> pretendLayout;
```

合成方法是：
 
1. 生成数组：周次 * 一周七天 * 一天十节课 * 一节课有几门安排。我们计算一下：    
假设一个学期二十周，没有课程重叠，这就是 20 x 7 x 10 x 1 = 1400 个单元。    
数据量确实很小，总体上耗时也是很均衡。所以理论上这是个 O(n4) 复杂度算法，实际上可以认为这是个 O(1) 复杂度算法，这个在接下来渲染时候更加体现。
2. 遍历每一周的每一天，进行插入课程操作。方法是对时间安排进行遍历，如果在这一天有安排，先将其安排到一个 `thisDay` 数组，然后对冲突处理后，插入到课程单元种
3. 关于课程冲突，也就是一个单元内有两个安排，以课程长度长的优先。在步骤中，先对 `thisDay` 数组进行排序，然后进行插入。
4. 剩下的单元，如果是空白，插入 -1 索引，表示不存在。

我解释完了，希望有个人帮我优化一下吧，我算法课成绩太差了:-P

目前想法是把后面那一堆简化掉，使用一个 Map 词典解决问题，也就是说：

```dart
typedef Map<int,List<int>> = WeekClassTable;
List<WeekClassTable> pretendLayout;
``` 

其中词典的 int 元素是一周中的第几节课，比如说，周三的第三节课就是 2*7+3 = 17，那它的索引就是 17。

##### 获取当前时间课次

主要是时间段计算，我有一个时间段列表。交替开始结束时间。

```dart
// Time arrangements.
// Even means start, odd means end.
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
  "20:30",
];
```


1. 首先，介于 8:20 到 20:35 之间的时间才进行计算。
2. 获取当前时间，然后在上面的数组中卡出时间在哪个之后。
3. 如果那个时间属于上课时间，就是在上课，进行相关课程查找，否则，就是在课间。在课间就要考虑下一节课是啥状况，是和上一节课相同还是下一节课。

#### 课表渲染

课表使用了 StatefulWidget 的原因是，课表渲染需要涉及到一些 View 里面相关的变量，我需要使用 initState 函数初始化，所以就这样了。虽然可以搞个 Stateless 组件，在它的初始化函数中初始化，但是保不齐将来我需要写啥保存页面状态，我就需要有状态了。

看过我上面的课程表图，可以发现，除了 AppBar ，我的课程表分成上面的周次选择列，和下面的课程表。除此之外，点开课程显示的课程信息又是一个组件。

这个组件里面定义了很多的常量，这里我不赘述。

##### 课表页面初始化

首先介绍三个 Controller ，其中前两个十分重要，因为涉及到页面切换：

```dart
late PageController pageControl;  /// 记录页面信息的控制器
late ScrollController rowControl; /// 滚动控制器
late BoxDecoration decoration;	 /// 一个 Container 的装饰配置信息
```

第一个 pageControl 涉及到 PageView ，这里就是课程表信息的页面，我们使用这个来方便换页。

第二个 rowControl 涉及到最上面的周次选择列，控制上面周次选择的滚动。

前两个控制器共享 currentWeekIndex 这个状态。

第三个 decoration 就是我课程表可以搞背景的东西，这个我不打算在博文里面说了，因为太简单了。

页面初始化，本质上就是这三个控制器的初始化了。首先判断当前应该显示第几周的课，然后分别使前两个控制器的初始值在对应的周次，最后初始化背景图（如果有的话）。

在判断显示周次上，如果当前不在上课周期，判断开学前还是刚放假，然后相应设置为第一周和最后一周。

##### 最上面的表列

这个是一列按钮，分别是周次按钮，和该周课表大致显示。

这个东西有个锁，叫做 `isTopRowLocked` ，保证按下按钮的时候数据的统一性，毕竟页面状态有两个控制器都在读。

每个按钮都有个函数，这个函数定义如下：


```dart
() {
	isTopRowLocked = true;
	setState(() {
	currentWeekIndex = index;
	pageControl.animateToPage(
		index,
		curve: Curves.easeInOutCubic,
		duration:
		const Duration(milliseconds: changePageTime),
	);
	changeTopRow(index);
	});
}
```

详情查看 `_topRow` 函数。

当按下按钮的时候，最顶部的锁锁上，然后刷新状态，这其中：

1. 设置页面信息为目标页面
2. pageControl 控制器进行换页操作，这其中有动画和动画时长。
3. 最上面表列进行换页操作，然后开锁。

其中上面换表列的操作比较复杂，因为不是 PageView，每次的偏移量需要提前算好，这也是我将换周次按钮的一些装饰信息写作常量的原因。

另外，为了适应横屏幕，尤其是手机窄屏幕的横屏幕，我设置了高度 500 px 限制，小于这个数时候，只显示文字，不显示大致课表概览。

##### 索引行

这一行，在代码里面叫 `_middleRow`，是用来显示日期信息的。这块代码有三处值得注意：

1. 需要计算那一周周一的日期。
2. 今天的颜色需要不一样。
3. 长宽比不同的时候，字体的颜色不同。

##### 课程表具体内容

课程表你可以发现有八列，最左面一列是显示数字索引的，这里不过多说明。右面七列就是课表了。

关于课表，希望大家还记得我上面说到的稀疏数组，那个数组实际上对应了这里。我们的渲染是按照周一到周日七天七列来处理的。

每一列都是由若干 classCard 生成的，classCard 需要三个变量：课程索引，课程长度，以及一个冲突课程 Set 。

```dart
Widget classCard(int index, double height, Set<int> conflict)
```

卡片根据索引来渲染：如果索引是 -1，我们认为这个地方没课，渲染一个空白的卡片占位；如果索引不是 -1，我们将直接渲染对应课程，同时引入一个按钮，在按下去的时候显示所有冲突课程的信息。卡片高度是基于课程表高度计算的，稍后我将介绍。

当渲染每一周的时候，我们查询在那个稀疏数组中对应的元素，然后提取出第一个元素，也就是给用户渲染的课程信息。然后决定长度，方法是向后遍历，并且累加循环标志变量和长度。这其中，所有在这个范围内的冲突课程都要记录下来，为防止重复信息，使用 Set ，也就是不重复序列。最后，不重复序列去掉 -1 元素，因为代表没有课程信息。

```dart
if (index != 0) {
	List<Widget> thisRow = [];

    // Choice the day and render it!
	for (int i = 0; i < 10; ++i) {
		// 提取出第一个元素
		int places = controller.pretendLayout[weekIndex][index - 1][i].first;

		// The length to render.
		int count = 1;
		Set<int> conflict = 
			controller.pretendLayout[weekIndex][index - 1][i].toSet();
        
        // 决定长度，向后遍历，并且累加循环标志变量和长度
        while (i < 9 && 
		controller.pretendLayout[weekIndex][index - 1][i + 1].first == 
		places) {
            count++;
            i++;
            conflict.addAll(
				controller.pretendLayout[weekIndex][index - 1][i].toSet()
			);
        }

        // 不重复序列去掉 -1 元素
        conflict.remove(-1);

        // Generate the row.
        thisRow.add(classCard(
          places,
          classTableContentHeight(count),
          conflict,
        ));
	}

	return thisRow;
}
```


最后说明课程表高度的计算。页面高度在 800px 是个节点，小于 800 的话，直接乘以 0.85 ，来隐去九十节课；大于 800 的话，页面高度减去上面两层的高度。
最后，我使用了 `SingleChildScrollView` 包裹整个课程表，让八列可以同时滚动，防止页面高度小于 800px 的情况。

##### 课程详细信息

代码在 `_buttomInformation`函数中，他接受那个冲突课程 Set 。根据这个 Set 提供的索引，输出对应课程的时间信息，和该课程安排在第几周生效。

(这块我是抄某个同学的，他还提醒我要写上课程序号啥的)

使用 `showDialog` 函数弹出信息，弹出的是一个 Column 列，总共是这个时间段内的所有课程。

##### 未安排课程信息

很简单地用新页面胡乱搓了个()代码很简单：

```dart
class NotArrangedClassList extends StatelessWidget {
  final List<ClassDetail> list;
  const NotArrangedClassList({super.key, required this.list});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("没有时间安排的科目"),
      ),
      body: dataList<Card, Card>(
        List.generate(
          list.length,
          (index) => Card(
            child: ListTile(
              title: Text(list[index].name),
              subtitle: Text(
                "编号: ${list[index].code} | ${list[index].number}\n"
                "老师: ${list[index].teacher ?? "没有数据"}",
              ),
            ),
          ),
        ),
        (toUse) => toUse,
      ),
    );
  }
}

```

### 横屏幕和竖屏幕

我的程序做了一点平板的优化，主要是我为了学 Flutter 响应式开发而搞出的副产品。

#### 如何在 Flutter 侦测横屏幕竖屏幕

Flutter 本身有很多的属性部件，比如 Theme 用来访问主题属性，Navigator 访问路由栈之类。这里我使用的是 MediaQuery.of(context).size，这是用来侦测当前页面长宽高状态的。实际上，上面我提到的很多高度检测啥的，都是用这个实现的。

而侦测屏幕位置，有两个思路：
 - 长宽比，长大于宽就是横着，否则就是竖着。
 - 之前我看到一篇文章说宽度 480 是个坎，小于算竖着。
我这里使用了后者的想法，前面的想法我就不写了：

```dart
bool isPhone(context) => MediaQuery.of(context).size.width < 480;
```

顺便说一句 LayoutBuilder， 是用来给部件加约束的组件，具体看官方指南吧。

#### 我的 BothSideView

先给大家看看这玩意到底是个啥东西：

![](https://legacy.superbart.top/picture/xdyou/both.side.sheet.gif)

如你所见，在竖屏的时候，他是从底往上呼出的，跟 [BottomSheet](https://m3.material.io/components/bottom-sheets/guidelines) 一样；在横屏的时候，他是从右向左呼出的，和 [SideSheet](https://m3.material.io/components/side-sheets/overview) 一样。

Flutter 的 Material 框架本身没有实现 SideSheet ，而对于横屏来说，BottomSheet 是十分浪费屏幕，而且不太好看，从左面呼出是更合适的。得亏有很多的大佬，自行实现了 SideSheet 插件，我可以直接拿来使用他们的概念，但我想把这两个结合在一起。

而为啥要将这两个东西合在一起呢？这就涉及到实际使用中，我们是如何呼出 BottomSheet 了。

呼出 BottomSheet 和呼出 Dialog 一样，是使用了一个函数，在这里，叫 `showBottomSheet`。这玩意有个问题，他本质上是往路由栈里面压入一个 BottomSheet 页面栈，也就是说，无论横屏幕还是竖屏幕，他永远是 BottomSheet，而不会变化一点。我一开始用了 SideSheet，结果发现横屏开了 SideSheet，竖屏过来了还是 SideSheet，他们之间不会互相转化。

那我就缝合吧，SideSheet 好办，抄过来先辈的代码就好了，顺便我抄过来使用 `showGeneralDialog` 来显示弹窗了。但是 BottomSheet 本身并没有任何代码资料，我只能自己写了。我使用了 StatefulWidget 来保存 heightForVertical 变量，这是个高度变量，默认为页面高度的 80% 。然后我使用了一个 GestureDetector ，手势侦测器。这个侦测器在拖拽最上面的小横杠时候进行当前高度检测，然后更新高度。这里我将收起的高度定为页面高度的 40% 。

这里我说明一下 BottomSheet 和 SideSheet 的特点，他们都可以分成两个部分，上面的和下面的。下面的是传参传进来的部件，上面的就是属于部件的东西了。

最后再说一句，原来的 SideSheet 的最上面是使用 `AppBar` 实现的，但是 AppBar 会侦测手机的状态栏，最终导致在某些情况下，上面的高度过高。我被迫自行实现了这里，搞得很难看。

现在我贴出来代码，欢迎改善完发个包：

```dart
import 'package:flutter/material.dart';
import 'package:watermeter/page/widget.dart';

class BothSideSheet extends StatefulWidget {
  /// child 是子部件，title 是标题，用于 SideSheet
  final Widget child;
  final String title;

  const BothSideSheet({
    super.key,
    required this.child,
    required this.title,
  });

  // 这里我是抄的那个 SideSheet 组件，他也是写了个静态方法来显示
  static void show({
    required BuildContext context,
    required Widget child,
    required String title,
  }) =>
      showGeneralDialog(
        barrierDismissible: true,
        context: context,
        pageBuilder: (context, animation1, animation2) {
          return BothSideSheet(
            title: title,
            child: child,
          );
        },
        barrierLabel: title,
		// 这个动画就是从右呼出还是从下面呼出
        transitionBuilder: (context, animation, secondaryAnimation, child) {
          return SlideTransition(
            position: Tween(
              begin: isPhone(context)
                  ? const Offset(0.0, 1.0)
                  : const Offset(1.0, 0.0),
              end: Offset.zero,
            ).chain(CurveTween(curve: Curves.easeOutCubic)).animate(animation),
            child: child,
          );
        },
      );

  @override
  State<BothSideSheet> createState() => _BothSideSheetState();
}

class _BothSideSheetState extends State<BothSideSheet> {
  // 这就是 BottomSheet 的高度问题了
  late double heightForVertical;


  // 这里涉及到 StatefulWidget 的声明周期，这是在 build 之前执行的函数，用来设定高度
  // 没记错的话，这么写的目的是，防止子组件的某些东西重新加载，这里我快忘了
  @override
  void didChangeDependencies() {
    heightForVertical = MediaQuery.of(context).size.height * 0.8;
    super.didChangeDependencies();
  }

  BorderRadius radius(context) => BorderRadius.only(
        topLeft: const Radius.circular(16),
        bottomLeft: !isPhone(context) ? const Radius.circular(16) : Radius.zero,
        topRight: isPhone(context) ? const Radius.circular(16) : Radius.zero,
        bottomRight: Radius.zero,
      );

  double get width => isPhone(context)
      ? MediaQuery.of(context).size.width
      : MediaQuery.of(context).size.width * 0.4 < 360
          ? 360
          : MediaQuery.of(context).size.width * 0.4;

  // 这就是上面的东西，根据 SideSheet 和 BottomSheet 有所不同
  Widget get onTop => isPhone(context)
      ? GestureDetector(
          onVerticalDragUpdate: (DragUpdateDetails details) {
            setState(() {
              heightForVertical = MediaQuery.of(context).size.height -
                  details.globalPosition.dy;
              if (heightForVertical <
                  MediaQuery.of(context).size.height * 0.4) {
                Navigator.pop(context);
              }
            });
          },
          child: Container(
            height: 30,
            width: double.infinity,
            color: Theme.of(context).colorScheme.surface,
            child: Stack(
              alignment: AlignmentDirectional.center,
              children: [
                Container(
                  color: Colors.transparent,
                  width: double.infinity,
                ),
                Container(
                  width: 32,
                  height: 4,
                  decoration: BoxDecoration(
                    borderRadius: BorderRadius.circular(15),
                    color: Theme.of(context)
                        .colorScheme
                        .onSurfaceVariant
                        .withOpacity(0.4),
                  ),
                )
              ],
            ),
          ),
        )
      : // 这里就是原先使用 AppBar 的地方，我这里被迫自己写了个类似的
		Container(
          height: kToolbarHeight,
          width: double.infinity,
          color: Theme.of(context).colorScheme.surface,
          child: Row(
            children: [
              IconButton(
                onPressed: () => Navigator.pop(context),
                icon: const Icon(Icons.arrow_back),
              ),
              Text(
                widget.title,
                style: Theme.of(context).textTheme.titleLarge,
              ),
            ],
          ),
        );

  @override
  Widget build(BuildContext context) {
    return Align(
      // 使用 Align 来侦测这个组件在右面还是底下
      alignment:
          isPhone(context) ? Alignment.bottomCenter : Alignment.centerRight,
      child: Container(
        width: width,
		// 页面高度
        height: isPhone(context) ? heightForVertical : double.infinity,
        decoration: BoxDecoration(
          color: Theme.of(context).colorScheme.surface,
          borderRadius: radius(context),
        ),
        child: Padding(
          padding: EdgeInsets.symmetric(
            horizontal: isPhone(context) ? 15 : 10,
            vertical: isPhone(context) ? 0 : 10,
          ),
          child: Scaffold(
            extendBodyBehindAppBar: true,
            appBar: isPhone(context)
                ? PreferredSize(
                    preferredSize: const Size.fromHeight(20),
                    child: onTop,
                  )
                : PreferredSize(
                    preferredSize: const Size.fromHeight(kToolbarHeight),
                    child: onTop,
                  ),
            body: widget.child,
          ),
        ),
      ),
    );
  }
}
```

#### PageView 组件使用

还是跟组件状态玩命。

原先，我的首页是抄的 [Flutter 的 M3 实例](https://flutter.github.io/samples/material_3.html)。这样我就可以在横屏幕时候使用左侧的 [NavigationRail](https://m3.material.io/components/navigation-rail/overview)，竖屏幕的时候使用底部的 [NavigationBar](https://m3.material.io/components/navigation-bar/overview)。

那么，问题在哪？我原先写的组件，将横屏渲染和竖屏渲染函数给分开写了。结果就导致前几天我迁移首页四个卡片到 PageView 的时候，出现了横屏和竖屏切换时候，页面永远会刷新到第一页。一开始我看了好久的 StatefulWidget 的状态周期，我也没明白。最后我发现，我这是两个组件，每次刷新的时候都会重新绘制这两个组件。解决方法就是，将这两个组件合二为一，在一个组件里面渲染，使用 `Visibility` 组件按需隐藏。

## 关于开源的想法

我对软件，按照开源和开发者，这么看：

> 个人开发的开源软件或半开源软件 > 集体开发的开源软件 > 个人开发的闭源软件 > 集体开发的半开源软件 > 集体开发的闭源软件

其中，半开源软件请参考 [FDroid 的负面特征定义](https://f-droid.org/zh_Hans/docs/Anti-Features/)。显然我的软件属于半开源软件，我这个软件实质上模拟了你在浏览器中，对学校后端的访问。

实际上软件的开源与否，并不重要，重要的是软件本身能不能很好用，而按照我的经验，软件的好用也可以这么排序，尤其是手机端应用()

所以，我虽然经常说开源很重要，但这个实际上是因为我认为个人开发者的产品更好而导致的。而开源软件放前面，是因为代码开放让人用着更舒服，可能我长期用 Linux 留下来的某种遗留症状。而且我某种意义上，真的不喜欢封闭的东西，虽然我发现大家都喜欢。

而为啥我要将这个软件按照 MPL 授权，是因为我的软件有很多可以复用的东西，比如上面我大幅度提到的课程表和那个 BothSide 。这些复用的东西我将来是打算做成程序内的 package，如果按照 GPL ，不利于传播。而我目前程序状态，如果使用 MIT 之类的，那可能会有很多的魔改版，然后闭源了。MPL 是按照文件强制开源的，就目前状态所言，假如你只是用了我的课程表代码文件，那么，你只需要开源课程表代码文件+你对这个代码的修改，就好了。
