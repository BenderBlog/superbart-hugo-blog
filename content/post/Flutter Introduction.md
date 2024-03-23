+++
title = "Flutter 介绍"
slug = "Flutter Introduction"
description = "介绍 Flutter ，被 React Native 罩住的玩意"
date = "2023-04-29"
image = "https://legacy.superbart.top/picture/Flutter%20Introduction/Maggie's%20Butterfly%20Longest%20Daycard%20Short.jpg"
categories = [
    "Technology"
]
tags = [
    "Flutter",
    "编程",
]
+++

有人邀请我去开个沙龙，我决定将这个，这个就是当时我的演讲稿。

## 什么是 Flutter & Flutter 的好处

Flutter 是一个跨平台的客户端(以及网络前端)开发工具，官方定义为：

> Flutter 是 Google 开源的应用开发框架，仅通过一套代码库，就能构建精美的、原生平台编译的多平台应用。

鉴于入门介绍，我就说的明白些。

1. 这玩意是用来写客户端程序的，也就是面向用户的程序。

2. 这个东西能够为很多平台生成应用，尽量做到了“平台无关”。

3. 这个东西上手比较简单，性能比较高，开发效率很高。

目前这个和 React Native 并列两大最流行的跨平台开发平台。而 React Native 还是占用了 React 前端开发框架（Flutter 受 React 影响很大）的优势，Flutter 相比之下就比较小众了，找工作不太好找:-P

对我而言，有了 Flutter 的基础，后面要适应其他的类似框架就方便多了。最近我被(zi)人(ji)拉(zhao)过(shi)去(qing)写 vue 去了，我之前没有接触过。但是我稍微看了一下 vue 组合式的教程，就能给人打下手了。CSS 我现在还不会，感觉要会了，我就又会了一个框架(逃)。

## Dart 语言介绍

Flutter 使用的是 Dart 语言，目前是 Google 专门为 Flutter 设计的语言，因为我根本没找到任何在其他方面用 Dart 编程的例子。而且这玩意曾经还想嵌入到 Chrome......

### Dart = Javascript + Java

语法像 Javascript，运行时环境像 Java。

像 Javascript 在于存在箭头函数，函数变量之类。Dart 对异步的实现 Future 也借鉴了 JS 的 Promise。因为 Dart 设计的时候，对标的就是 JavaScript。

而运行环境像 Java，因为他的类设计，编译和运行也很像 Java。类的方面下面会说明。

Dart 代码的运行有三种方式：一种是直接解释，一种是转码成  Javascript ，一种是编译成 DartVM 虚拟机机器码，然后在 DartVM 里面运行。最后一种有一种 Java VM 的既视感讲道理:-P

上面三种方式对应了 Flutter 的开发：调试开发，网页开发，客户端程序。

### 给点例子吧

#### 基本

```dart
// Dart 是强类型语言，但是支持类型推断
var lineCount;
var count = 1;

// 循环，判断和 C 和 JS 一样
while (count > 0) {
    if (weLikeToCount) {
        lineCount = countLines();
    } else {
        lineCount = 0;
    }
    count--;
}

print(lineCount);
```

#### 函数

```dart
// 普通函数
int fibonacci (int n) {
    if (n == 0 || n == 1) return n;
    return fibonacci(n-1) + fibonacci(n-2);
}

// 箭头函数(和 JS 的有点区别)
int fib (int n) => (n == 0 || n == 1) ? n : fib(n-1)+fib(n-2);

// 另一个使用例子 (e) => print(e))
List<int> nums = [2,0,2,2,0,4,2,8];
nums.forEach((e) => print(e));
```

#### 类

这玩意东西太多了，我就光码字吧：

- 类的成员默认都是公共成员，私有成员是在变量名前加 `_`号，有`@protected`宏。

- Dart 的类是单向继承，支持接口类

- 支持 abstract 抽象类，也就是需要继承来实现的类

#### 异步方法

先来个定义

> 异步是在很多领域都有的概念，在编程中，是相对于同步的。同步就是一条指令一条指令，按顺序执行。异步则可以同时运行多个任务，执行任务的时候，可以先返回一个“包含进度的实例”。然后有“回调函数”来把该实例中执行的状态返回。

Dart 的异步叫 `Future<T>`，其中 T 是泛型啦。当你运行异步方法的时候，他会先返回一个 Future 类，然后按需返回结果，或者处理结果。我们有两个方式处理异步编程：

```dart
const oneSecond = Duration(seconds: 1);
Futur<void> printWithDelay(String message) async {
  await Future.delayed(oneSecond);
  print(message);
}
```

相当于下面这段代码：

```dart
const oneSecond = Duration(seconds: 1);
Future<void> printWithDelay(String message) {
  return Future.delayed(oneSecond).then((_) {
    print(message);
  });
}
```

#### 空安全

在你们使用 C 语言变量的时候，经常出现变量尚未定义就被使用了。Dart 引入了空安全机制，来帮助避免这个现象，让代码更稳定。

```dart
// 默认所有类型均不可空，类型加问号，表示该变量可空
int? a = null;

// 如此写会报编译错误，语言会进行空检查的
int a = null;

// 可以使用 late 表示稍后赋值，但你不能忘了
late int a;
```

当然还有很多，想知道的话请去看官方介绍。我当时看了俩下就上手了......

## Flutter 的基本部件介绍

Flutter 的 Widget 是一个一个的类，描述了在当前的配置和状态下视图所应该呈现的样子。在 Flutter 里面，万物都是围绕部件旋转的。

接下来我要展示一个信息卡，用这个方式给大家展示 Flutter 的基本组件。顺便我搞点 HTML 之类的东西，来给大家做点对比。接下来的部件，都是按照 Material 部件来说明的，iOS 的不在此说明。

### Text 部件

Text 是用来渲染一段文字的。

```dart
Text("Maggie Rules!");
```

效果大致说，跟HTML的这个一样。

```html
<p>Maggie Rules!</p>
```

Text 的属性有很多，比如说大小，斜体之类。有一个类叫 TextStyle，来给Text加属性，比如字体，阴影，颜色之类。那么，我可以这么写一个绿色的字。

```dart
Text("50 sucks",size:14,style:TextStyle(color: Colors.green));
```

效果大致说，跟HTML的这个一样。

```html
<p style="color: green"> 50 sucks </p>
```

我感觉通过这个，你们知道这玩意和 HTML+CSS 的对应了吧，也许。

### Row，Column，Warp

你们可以看到，我在这些卡片上画了几条线。这是为了说明我们设计该卡片的基本架构，行和列。Flutter 的部件构造，就是在 Row 和 Column 之上的。

Row 和 Column 的写法差不多，都是这样的，更多属性一会再说：

```dart
Row(children:<Widget>[]);
Column(children:<Widget>[]);
```

Row 代表行，Column 代表列。我们这个卡片是有三行的，每行是有对应元素的。通过这个，我们可以写出这个东西的框架了。

我们先实现每一行，第一行是在两侧的两个元素，注意到中间很大间隔了吗？这个是 Row 的一个属性，AxisAlignment。

AxisAlignment 是指这个部件两个轴上部件的排列方式，分为主轴 MainAxisAlignment 和交叉轴 CrossAxisAlignment。这张图片显示出这两个部件的主轴和交叉轴。我们通过修改这个，来规划好在该列/行上元素的排列方式。对于第一行，我们是这样写的：

```dart
Row(
    mainAxisAlignment: MainAxisAlignment.center,
    children:<Widget>[
        //第一个Text
        //第二个Text
    ],
);
```

剩下两行我这里就不赘述了，他们的排列方式都是靠左，也就是默认值。大致的代码如下：

```dart
Column(
    children:[
        Row(
            mainAxisAlignment: MainAxisAlignment.center,
            children:<Widget>[
                Text("第1次"),
                Text("打卡成功")
            ],
        ),
        Row(
            children:<Widget>[
                Icon(Icons.pumch_clock),
                Text("2022-12-12 11:11:11")
            ],
        ),
        Row(
            children:<Widget>[
                Icon(Icons.error),
                Text("打卡成功")
            ],
        ),
    ],
);
```

以上部分是最基本设计Flutter布局的样例了。实际使用中，这样写的方式很死板，遇到一些动态变化的组件，比如说很多行的文字，Column高度侦测问题等等，会花费大量的时间设置这些东西的样式。所以，在实际PDA的编写中，我是使用了Warp来让其自动排列这些组件，你只是需要输入这些部件就好了。

```dart
Wrap(
     alignment: WrapAlignment.spaceBetween,
     children: [
        TagsBoxes(
              text: "第 $mark 条",
              backgroundColor: Theme.of(context).primaryColor,
            ),
            situation(),
            const Divider(
              color: Colors.transparent,
              height: 5,
            ),
            informationWithIcon(Icons.punch_clock,
                "${toUse.punchDay}-${toUse.punchTime}", context),
            informationWithIcon(Icons.place, toUse.machineName, context),
            if (!toUse.state.contains("成功"))
              informationWithIcon(
                  Icons.error_outline,
                  toUse.state.contains("锻炼间隔需30分钟以上")
                      ? toUse.state.replaceAll("锻炼间隔需30分钟以上", "")
                      : toUse.state,
                  context),
          ],
        ),
```

其中 InformationWithIcon 是这样的：

```dart
Widget informationWithIcon(IconData icon, String text, context) => Row(
      children: [
        Icon(
          icon,
          size: 14,
          color: Theme.of(context).colorScheme.tertiary,
        ),
        const SizedBox(width: 5),
        Expanded(
          child: Text(text),
        ),
      ],
    );
```

TagsBoxes 需要在 Container 讲明白了之后才能说明。

### Container

Container是一个拥有绘制、定位、调整大小的 widget，是开发中最常用、最基础的组件。顾名思义，他能包装很多的组件。地位类似于 HTML 的 div。

上面的组件，如果我要成为一个个卡片，我得用这个包装：

```dart
Container(
    child: Wrap(/* 上面展示的东西 */),
)
```

对于 Container，我们需要引入一些对于有些人很熟悉的东西，也就是说，Margin 和 Padding，外边距和内边距。对于 Container 而言，内边距用到的最多。我们还可以设置这玩意的边框，圆角，背景颜色之类。扩展完相当于这样：

```dart
Container(
    padding: const EdgeInsets.all(15),
    decoration: BoxDecoration(
        borderRadious: BorderRadius.all(Radius.circular(10)),
        color: Color.purple,
    ),
    child: Wrap(/* 上面展示的东西 */),
)
```

类似于这个：

```html
<div
    style="background-color:purple;border-radius:10%;padding:15px"
>
xxx
</div>
```

实际上 Container 是很多部件的最终实现方式，比如 Card，他就说按照设计规范，设计好背景颜色，边框圆角，背景颜色之类。除此之外，还有强制设定长宽的 SizedBox，强制设定装饰的 DecortatedBox 等，都可以算 Container 的扩展。实际代码中，我直接把上面提到的 Warp 套进 Card 了。

最终，我说明一下上面说到的 TagBoxes。代码是这样的：

```dart
Container(
  padding: const EdgeInsets.fromLTRB(5, 3, 5, 3),
  decoration: BoxDecoration(
    color: backgroundColor,
    borderRadius: const BorderRadius.all(Radius.circular(9)),
  ),
  child: Text(
    text,
    style: TextStyle(color: textColor),
    textScaleFactor: 0.9,
  ),
)
```

### ListView

卡片介绍就这样了，在实际情况下，我们会有超级多的记录。根据思维惯性，我们会想让其做成一个可以滚动的菜单。不过不能用 Column，因为单纯的 Column 缺少滚动侦测器，也就是说，我们缺少一个侦测目前该滚动菜单滚动位置的侦测器。所以，我们需要使用 ListView 部件，他默认有一个滚动侦测器。

```dart
ListView(
    children:[
        Card(xxx),
        ......
    ],
)
```

滚动侦测器涉及到接下来要说的状态管理。

### Scafford



Material 设计的页面部件框架，包括但不限于：

- appBar：上面的导航栏（可以设置标题和右面的小按钮，称为 action）

- tabBar：一个框架的分页，分页内容另有设置

- body：页面的主要部分，对于截图是打卡记录

- bottomNavigationBar：底部的导航栏，对于截图是展示次数以及转换

## Flutter 内部的状态管理

### 声明式编程

我先念一段上网找到的定义：

> 命令式编程就像它的名字一样，它由开发者我们一步一步的告述计算机，执行一系列的操作，然后得到想要的结果，起主要作用的是开发者，计算机只是帮助开发者执行计算而已。我们日常使用的大多数语言都属于命令式。
> 
> 而声明式编程却与此相反，它不是告述计算机做什么做，而是直接告述计算它想要的结果，至于怎么做，由预先写好的程序依据一定的算法由计算机自动推算出来。这类定义比如 SQL，Vue 的响应式组件。

官方给了个这个公式：

>  UI = f(state)

Flutter 部件的构造过程，如这个公式所见，是这样的：

我们有一个UI，或者说部件，的构造函数，里面写好了这个部件需要接收，或者监听的状态。我们通过创建，修改这个状态，让程序组建/更新我们的部件。这个状态就是我们希望的结果。这说起来十分拗口，我们上两个例子。

### StatefulWidget 内部管理和 setstate

之前我们提到的部件，都是 Stateless 部件，也就是说，这个部件的状态不会变，在我们一开始渲染的时候，就写死了。

但是，状态有时候是需要更新的。比如说，最开始那个计数器应用，我们需要记下来目前数字是多少，并且我们需要能响应添加和减少。鉴于这个，我们需要引入 StatefulWidget 来实现这个。

StatefulWidget 依靠 setState 来刷新部件，我们看一下计数器代码。

```dart
import 'package:flutter/material.dart';

// 所有应用的入口
void main() {
  runApp(const MyApp());
}

// 这些都是定义框架的
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

// StatefulWidget 可以通过输入 stful 来快速生成
// StatelessWidget 通过输入 stless 来生成
class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  // 所有在 Widget 里面的东西都是 final
  final String title;

  // 状态在此生成
  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  // 注意里面的 setState，他是用来更新部件状态的
  // 里面的函数就是状态是如何被更新的了
  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  // 每当 setState 运行，部件状态被更新，这个函数会重新运行
  // 更新适应这个状态的部件
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        // 这里看不懂，建议看上面的组件
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        // 注意这里，这个按钮按下的时候，会执行这个函数
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ), 
    );
  }
}
```

StatefulWidget 适合于一个小部件内部短时状态的维护。如果我们要搞牵扯到许多部件，乃至于各个页面的共同状态，就很难办了。这里我要给大家介绍一个我日常在使用的状态管理器：GetX。

### GetX

GetX 是三个库的集合：状态管理，路由管理，和依赖管理。这里只关注状态管理。

#### GetX 观察者模式状态管理

第一个状态管理使用的是obs->观察者模式，我们记住这么几点：

- 在变量初始化的时候，初始化值的后面添加`.obs`来使其可观察化

- 使用`Obx`部件来渲染需要用到可观察化变量的部件

- 使用平常的方法修改可观察化变量的值

比如这个计数器应用：

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';

void main() {
  runApp(const MainApp());
}

// 在 GetX 中，给变量添加 .obs 就可以使其被观察
// 这时，他的类型不再是值的类型了
var count = 0.obs;

class MainApp extends StatelessWidget {
  const MainApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        // 注意这里 Obx 部件，他能获取对应的可观察部件
        // GetX 保证这个寻找是相当快的
        appBar: AppBar(title: Obx(() => Text("Clicks: $count"))),
        body: Center(
          child: TextButton(
            onPressed: () {
              // 这里没用 setState，仅仅对该可观察变量里面的值修改即可
              count.value += 1;
            },
            child: const Text(
              "Add it!",
              style: TextStyle(
                fontSize: 18,
              ),
            ),
          ),
        ),
      ),
    );
  }
}
```

#### GetX 控制器类状态管理

再给大家介绍一下`GetxController`，我 PDA 用的后者更多。

我先给大家介绍 MVC 架构，Model 模型，是说程序的功能。控制器是和视图View进行交流，View视图就是显示了。View 通过 Controller 获取 Model 中的东西。

每个 GetX Controller 都是继承 GetController 虚拟类的一个类。这个类里面，除了你要使用到的值和方法，还有两个方法：

- onInit()：在这个控制器初始化的时候使用。

- onReady()：在这个控制器刚初始化（时间大约一帧后）运行，处理异步请求。

在使用控制器的时候，我们可以直接用：

```dart
GetBuilder<Controller>(builder: (c) => Widget(xxx));
```

建议阅读 Traintime PDA 代码中的`controller/sport_controller.dart`，`repository/xidian_sport/xidian_sport_session.dart`，以及 page 下面关于体育部件的代码。以下这段展示的是我程序对应的模型各组成的部分。

> Controller(GetX Controller) -- Model(Dio网络库) -- View(Flutter)

## 杂项

### 路由栈

栈是先进后出的结构，而路由栈里面，存的是每个页面的信息了。在 Flutter 中，我们这么处理路由栈：

```dart
// 这个是说压入路由栈，进入这个页面。
Navigator.of(context).push(route/widget);
// 这个是说弹出路由栈的顶，也就是返回上一个页面
Navigator.of(context).pop();
// 可以做到按需弹栈，然后压栈
Navigator.of(context).pushNamedAndRemoveUntil(
  route/widget,                      // 弹栈之后要压入的
  (Route<dynamic> route) => false,   // 这个是判断栈顶元素是否符合要求的函数
);
```

这里面的 context 是指，这个应用，或者这个部件的状态。

### Dio 网络插件

Flutter 提供了很多的插件，来方便我们的开发体验。其中最著名的就是 Dio 网络库。他是一个异步网络访问库，使用方式和 axios 比较像。

先说明一下拦截器，它可以在获取回复/发送请求时，先拦截之，然后对该包进行修改。

Dio 类的定义，其中我用到了拦截器和对基地址的设置，设置了这个，后面的访问就可以输入那个网站的子路由了。

```dart
/// Maybe I wrote how to store the data is better.
Dio get _dio {
  Dio toReturn = Dio(BaseOptions(
    baseUrl: _baseURL,
    contentType: Headers.formUrlEncodedContentType,
  ));
  // 这个拦截器是 Cookie 管理器
  toReturn.interceptors.add(CookieManager(SportCookieJar));
  return toReturn;
}
```

Dio 的使用示例，它可以支持 POST，GET 等常见的 HTTP 请求方式。可以设定传输参数，请求头等很多东西。它的返回和 axios 大致相同，有响应数据，响应代码等。

```dart
String termStartDay = await dio.post(
  'https://ehall.xidian.edu.cn/jwapp/sys/wdkb/modules/jshkcb/cxjcs.do',
  data: {
    'XN': '${semesterCode.split('-')[0]}-${semesterCode.split('-')[1]}',
    'XQ': semesterCode.split('-')[2]
  },
).then((value) => value.data['datas']['cxjcs']['rows'][0]["XQKSRQ"]);
```

### 存储

#### Dart 操作文件的函数

```dart
// 这样定义一个文件
var file = File('file.txt');

// 按照字符读取文件的方法，异步和同步
Future<String> readAsString({Encoding encoding: utf8});
String readAsStringSync({Encoding encoding: utf8});

// 按照一行一行字符读取文件的方式，异步和同步
Future<List<String>> readAsLines({Encoding encoding: utf8});
List<String> readAsLinesSync({Encoding encoding: utf8});

// 二进制读取方法
// Dart 中表示二进制有一个专门的类型，叫做 Uint8List
Future<Uint8List> readAsBytes();
Uint8List readAsBytesSync();

// 二进制写入方法
Future<File> writeAsBytes(List<int> bytes,
      {FileMode mode: FileMode.write, bool flush: false});
void writeAsBytesSync(List<int> bytes,
      {FileMode mode: FileMode.write, bool flush: false});

// 字符串写入方法
Future<File> writeAsString(String contents,
      {FileMode mode: FileMode.write,
      Encoding encoding: utf8,
      bool flush: false});
void writeAsStringSync(String contents,
      {FileMode mode: FileMode.write,
      Encoding encoding: utf8,
      bool flush: false});
```

#### path_provider

作为一个跨平台的开发框架，Flutter 要能适应很多方面，其中最主要的就是存储位置。我们要存储一个文件的时候，需要在不同设备上，找到对应的位置。而在很多设备上，相同类型文件的存储地方是不一致的。`path_provider`能够让我们找到相应的位置。具体使用方式请参阅它的文档。

以下这个表格能体现出存储地方不同的问题：

| Directory                    | Android | iOS | Linux | macOS | Windows |
| ---------------------------- | ------- | --- | ----- | ----- | ------- |
| Temporary                    | ✔️      | ✔️  | ✔️    | ✔️    | ✔️      |
| Application Support          | ✔️      | ✔️  | ✔️    | ✔️    | ✔️      |
| Application Library          | ❌️      | ✔️  | ❌️    | ✔️    | ❌️      |
| Application Documents        | ✔️      | ✔️  | ✔️    | ✔️    | ✔️      |
| External Storage             | ✔️      | ❌   | ❌     | ❌️    | ❌️      |
| External Cache Directories   | ✔️      | ❌   | ❌     | ❌️    | ❌️      |
| External Storage Directories | ✔️      | ❌   | ❌     | ❌️    | ❌️      |
| Downloads                    | ❌       | ✔️  | ✔️    | ✔️    | ✔️      |

以下是我程序的一份示例：

```dart
// Loading cookiejar.
// 先获取到 ApplicationSupport 的位置在哪
Directory supportPath = await getApplicationSupportDirectory();
// 注意 supportPath.path，这里我读取了路径结果
SportCookieJar = PersistCookieJar(
    ignoreExpires: true, storage: FileStorage("${supportPath.path}/sport"));
IDSCookieJar = PersistCookieJar(
    ignoreExpires: true, storage: FileStorage("${supportPath.path}/ids"));
```

#### shared_preferences

我们程序更多的是要在本地存储一些简单的设置信息，具体来说，是很简单的 key-value 东西了。比如说，你的学号和密码是什么，你的宿舍号之类。我们使用 shared_preferences 来解决这个问题。

```dart
// 初始化一个示例
final SharedPreferences prefs = await SharedPreferences.getInstance();

// 设置一份 key-value
// String 可以改成 Int, Bool, Double 等
await prefs.setString(key as String, value as String);

// 读取一份 key 对应的数据
// String 可以改成 Int, Bool, Double 等
String? v = prefs.get(key as String);

// 清除所有设置
prefs.clear();
```

## 推荐阅读

[Dart 语言官方简介](https://dart.dev/language)  
[Flutter 上手教程](https://flutter.cn/docs/get-started/codelab)  
[布局构建教程](https://flutter.cn/docs/development/ui/layout/tutorial)  
[Traintime PDA (Watermeter) 代码](https://github.com/BenderBlog/watermeter)  

## 封面
Maggie 去日托所的一天。

主要看中了蝴蝶，因为蝴蝶和 Dart 的吉祥物蜂鸟类似。