+++
title = "关于西电一站式服务大厅背后的那点事"
slug = "The things behind Xidian E-Hall"
description = "为啥我看不了我成绩的组成？"
date = "2022-08-31"
image = "https://legacy.superbart.xyz/picture/Random/Coding%20to%20a%20cure%20girl.jpg"
categories = [
    "Technology"
]
tags = [
    "抓包"
]

+++

# 关于如何使用西电一站式服务大厅

好吧，这个假期闲得慌想用 Flutter 把电表重写了(毕竟本人能力很差，就很容易瞎想)。目前开发还是没个身影，但是我感觉，我把核心功能给写出来了，就差套个壳。我觉得，核心功能就是从学校服务器上当下来我需要的数据。根据[我项目的简介](https://github.com/BenderBlog/watermeter)，应该是开发完大半了。

但是呢，鉴于本人的鸽子属性，我感觉我得在我将来大概率弃坑之前，留点材料，以方便将来有比我更无聊，更疯狂的人来完成他心目中的电表。

## 关于我校一站式服务大厅

我们学校的一站式服务大厅是由南京金智教育开发的，而且很多学校都用这个系统。所以，接下来的部分，感觉很多同志们都能适用。

我们学校的一站式大厅有很多功能，不过其中仅仅有很少的一部分我们能用。我感觉平常使用最多的就是：
* 课表
* 成绩查询
* 考试安排

而我们平常要使用一站式(或者其他东西，比如健康报告)的话，一般都要先登陆统一认证平台，之后才能进入一站式。而这个统一认证平台，也是金智的:-P

## 提前说明
1. 以下使用的语言是 Dart，编写 Flutter 应用的语言。个人觉得还算好理解吧......
2. 下面步骤，会多次出现本人故意暂停跳转情况发生。因为我发现，要是自动跳转，可能 Cookie 会存不下来，要是 python 的 request 库就没有这个问题。

## 如何登陆统一认证平台

### 工具
根据 [xidian-script](https://github.com/xdlinux/xidian-scripts) 姐妹计划 [libxduauth](https://github.com/xdlinux/libxduauth) 所说，登陆所需要的工具如下：
1. Cookie Jar。
2. 网络传输工具，其中需要设置 `application/x-www-form-urlencoded` 来传输参数。
3. 网页分析工具，比如大名鼎鼎的 BeautifulSoup 库。

### 操作步骤

#### 获取登陆网页
向 http://ids.xidian.edu.cn/authserver/login 发送 GET 请求，其中请求参数是:
```json
{
    "service": 接下来要访问的网址,
    "type": "userNameLogin"
}
```

如果正常，会返回登录网页。

将登陆网页交给网页分析工具，让他找到网页中 id 为 pwdFromId 的元素们，这里记为 form：
```Dart
/// Import 'package:beautiful_soup_dart/beautiful_soup.dart' before use.
var page = BeautifulSoup(response);
var form = page.find("form",attrs: {"id": "pwdFromId"});
```
#### 检查是否需要验证码
向 http://ids.xidian.edu.cn/authserver/checkNeedCaptcha.htl 发送 GET 请求，请求参数是
```json
{
    "username": 填入你的学号,
    "_":目前时间戳
}
```

查看返回值中有没有 	`true` 。

如果需要填入验证码的话，向 http://ids.xidian.edu.cn/authserver/getCaptcha.htl 获取验证码图片，请求参数是
```json
{ 目前时间戳: "" }
```

#### 加密密码

首先，我们需要填充密码。这里我们使用 PKCS7 填充方式。我的程序自己实现了，因为没看明白 Dart 里面 PKCS7 咋用的:-(：
 * 由于加密方式是 AES，要求分组长度是 128 bytes，根据 1 bit = 8 bytes，需要字符串长度是 16 的倍数。
 * 先在密码字符串前面插上 xidianscriptsxdu 四遍(正好是 16 长的字符串，Dart 默认给的是随机字符串)，然后将字符串转换成 int 数组(也就是把每个字符转换成对应的 ASCII 码)。
 * 然后计算我们还需要在后面插入多少元素来满足 16 的倍数，差几个在后面插入几个。插入内容为插入元素的个数。注意，要是一个都不差，也得插入数据，来保证数据和插入值都存在。
 * 由于接下来加密需要的是数字数组，所以不变回字符串。

然后，从 form 里面寻找 input 标签，且 id 为 pwdEncryptSalt 的元素，这里面是加密密码的密钥(还是盐啥的)。最后，使用 AES-CBC 算法加密，然后返回字符串。

上述步骤的具体代码如下：
```Dart
/// Get base64 encoded data. Which is aes encrypted [toEnc] encoded string using [key].
/// Padding part is copied from libxduauth"s idea.
/// Import "package:encrypt/encrypt.dart" before use.
String aesEncrypt(String toEnc, String key) {
  dynamic k = Key.fromUtf8(key);
  var crypt = AES(k, mode: AESMode.cbc, padding: null);
  /// Start padding
  int blockSize = 16;
  List<int> dataToPad = [];
  dataToPad.addAll(utf8.encode("xidianscriptsxduxidianscriptsxduxidianscriptsxduxidianscriptsxdu$toEnc"));
  int paddingLength = blockSize - dataToPad.length % blockSize;
  for (var i = 0; i < paddingLength; ++i) {
    dataToPad.add(paddingLength);
  }
  String readyToEnc = utf8.decode(dataToPad);
  /// Start encrypt.
  return Encrypter(crypt)
      .encrypt(readyToEnc, iv: IV.fromUtf8("xidianscriptsxdu"))
      .base64;
}
```

#### 发送登陆报文
首先，构建登陆请求所需要的头。初步需要三个信息：
 * username：你的学号
 * password：加密过的密码
 * rememberMe：防止之后又走一遍登陆过程

在此之后，我们需要寻找很多预配置的头，加入到我们的头中。这些元素都在 form 中，是 input 标签 ，参数是 `type=hidden` 。循环将其插入到请求头中。

最后，向 http://ids.xidian.edu.cn/authserver/login 发送 POST 请求，其中请求头是上面搞过的请求头，请求参数是 `{"service": 接下来要访问的网址}` ，请求不要自动跳转。

如果正常，ids.xidian.edu.cn 下面的 Cookie 会多一个 `happyVoyagePersonal=...; Path=/personalinfo`。然后需要我们自己向需要跳转的新网址发 GET 请求，同样，不需要自动跳转。

到此，通过统一认证平台，我们登陆了一站式服务大厅。

## 使用一站式服务大厅

首先给个表格：

| 应用名称 |  内部序号(appID)   |
| ------- | ---------------- |
| 课表     | 4770397878132218 |
| 成绩     | 4768574631264620 |
| 考试安排 | 4768687067472349 |
| 个人信息 | 4585275700341858 |


### 如何进入应用

1. 验证是否登录。向 http://ehall.xidian.edu.cn/jsonp/userFavoriteApps.json 发送 GET 请求，查看返回的数据中，hasLogin 是否为 true。如果没登录，则需要登录。登陆需要的 target 是 http://ehall.xidian.edu.cn/login?service=http://ehall.xidian.edu.cn/new/index.html。
一站式登录成功的话，ehall.xidian.edu.cn 下面的 Cookie 会多一个 `MOD_AUTH_CAS=MOD_AUTH_ST-...; path=/;`。
2. 请求访问应用。向 `http://ehall.xidian.edu.cn/appShow` 发送 GET 请求，其中请求参数是 `{"appId": 要访问应用的内部序号}`，header 追加参数如下：
```json
{"Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8",}
```
3. 暂停自动跳转请求，截断下来要跳转的网址，去相应函数请求，如果那个功能必要的话。

### 提前说明第二发
发送信息中如包含学期信息之情况，按照如此处理：
> 2022-2023-1 == 2022-2023 学年第一学期

虽然是可以通过请求得到学期数据，不过有的地方，我打算自己算。所以如有对应需求，请查看 xidian-script 代码。

### 获取成绩(学校想让你看到的)
一会再告诉大家为啥是"学校想让你看到的"。先给大家介绍原理。

#### 获取数据

在获取数据前，还记得那个要跳转的网址吗？你一定要给那个网址发个 GET 请求，要不然，接下来的步骤报错 403 未授权。

实际上这玩意本质上，就是向 http://ehall.xidian.edu.cn/jwapp/sys/cjcx/modules/cjcx/xscjcx.do 发送 POST 请求。不过发送的请求数据，要看情况。

首先说的是一个共有的请求数据，如下：
```json
{
  "name": "SFYX", //是否有效
  "value": "1",
  "linkOpt": "and",
  "builder": "m_value_equal"
}
```
然后，如果你想查看某个学期的数据，还需要追加以下请求数据：
```json
{
  "name": "XNXQDM", //学年学期代码
  "value": "2022-2023-1",//学期学年代码，参考提前说明第二发
  "linkOpt": "and",
  "builder": "equal"
}
```

然后，由于是 POST，所以没有请求数据设置(有错请指正)。所以我们附在 POST 报文的数据是：
```json
data: {
  "*json": 1,
  "querySetting": 将上面设置的请求数据编码为 json 字符串,
  "*order": "+XNXQDM,KCH,KXH",
  "pageSize":1000,
  "pageNumber": 1,
},
```
然后返回的数据，基本上是坑爹的汉语拼音缩写，感觉大家应该会破译吧。毕竟人均黑虎阿福，都是会乌鸦坐飞机的。

#### 关于那些坑爹的拼音缩写
我打开过网页的开发者模式，看到网络菜单有个请求，是向 https://ehall.xidian.edu.cn/jwapp/sys/cjcx/modules/cjcx.do 的 POST 请求。发送的也和上面的头差不多。然后他返回的信息，就是那些坑爹缩写的完整含义。实际上有好多，这里我只列出和查成绩相关的东西。

|   缩写    |     含义      |
| -------- | ------------ |
| XNXQDM   | 学年学期(代码) |
| KCM      | 课程名        |
| KCH      | 课程号        |
| XSKCM    | (学生)课程名   |
| XSKCH    | (学生)课程号   |
| ZCJ      | 总成绩        |
| KCLBDM   | 课程类别       |
| KCXZDM   | 课程性质       |
| XGXKLBDM | 校公选课类别   |
| XF       | 学分          |
| XS       | 学时          |
| XDFSDM   | 修读方式       |
| SFZX     | 修读类型       |
| XFJD     | 绩点          |
| JDF      | 积点分(绩点分) |
| JXBID    | 教学班id      |
| CXCKDM   | 重修重考       |
| SFJG     | 是否及格       |
| SFYX     | 是否有效       |

#### 为啥是"学校想让你看到的"
我发现，正常情况下，只有大一新生才能看到自己成绩的排名和具体组成。我查看了页面的源代码，在 `top/ehall.xidian.edu.cn/jwapp/cjcx/*default/modules/cjcx/dqxq/dqxq.js?av=一个时常变化的数` 文件中，有以下几行：
```js
/// The tenth line.
var isDyxs = false;//是否为大一学生（默认为否）西电大一学生可以查看成绩详情
/// The thirty-eighth line and below.
checkIsDyxs : function(){
    isDyxs = false;
    var dqxnxqdm = '';//2017-2018-1
        var xznj = '';//2017
        if("90001" == jwlcgl.getRZLB(roleId)){
            var url = WIS_EMAP_SERV.getAbsPath('/modules/cjcx/cxxsjbxx.do');
            var res = BH_UTILS.doSyncAjax(url, null);
            if(res.code=='0' && res.datas.cxxsjbxx.extParams.code=='1'){
                var xznj = res.datas.cxxsjbxx.extParams.msg;
                if("1" == xznj){
                    isDyxs = true;
                }	
            } else {
                $.bhTip({content: '学生现在年级查询失败，请稍后再试...', state: 'danger'});
                return false;
            }
        }
    },
```
要想看倒也容易，在网页下载完这个文件但还没加载之前，改掉这俩地方。具体各位可以上网搜"如何在 Chrome 中修改网页代码"。或者使用 Charles 或者 mitproxy 搞中间人拦截，手动改包。

另外说明，这个文件感觉是包含了所有跟查成绩相关的代码，要想深入了解的话可以看看。我对这玩意居然没有加密混淆表示无法理解。

最后我想问的是，这是什么高年级歧视。如果是要”保护“老师的话，不如想想为啥我们学生会对某些老师敬而远之。

### 获取课表数据

一般来说，获取到的数据是需要处理的。xidian-script 是处理成 iCalender 文件，一个国际上用来处理日历和备忘录的标准格式。我的程序计划是利用 Dart 的一个库，保存成 iCalender 。然后我在网上找到个课表的实现，试着套一下。

#### 当前学期信息

没错，这回我不打算自己合成了:-P

给 http://ehall.xidian.edu.cn/jwapp/sys/wdkb/modules/jshkcb/dqxnxq.do 发送 POST 请求。如果成功的话，在回复数据中的 `['datas']['dqxnxq']['rows'][0]['DM']` 中，就会包含这个学期的数据，格式见上文的提前说明。

感觉其他类似的应用应该有类似的方式来获取目前的学期，或者是所有的学期号码。

#### 获取开学日期

给 http://ehall.xidian.edu.cn/jwapp/sys/wdkb/modules/jshkcb/cxjcs.do 发送 POST 请求，发送的数据如下：
```json
{
    "XN": '学期代码的第一个数字-学期代码的第二个数字', //学年
    "XQ": 学期代码的最后一个数字, // 学期
},
```
获取的日期字符串在返回数据的 `['datas']['cxjcs']['rows'][0]["XQKSRQ"]` 中。

这个东西感觉是为了在显示课表对应日期的时候，找个基准。

#### 获取课表初步数据
给 http://ehall.xidian.edu.cn/jwapp/sys/wdkb/modules/xskcb/xskcb.do 发送 POST 请求，发送数据如下：
```json
{"XNXQDM": 当前学期信息字符串}
```
然后从 `['datas']['xskcb']` 提取信息。

查看 `['extParams']['code']` 是否为 1。如果是的话，从 `['rows']` 提取数据，否则，从 `['extParams']['msg']` 查看错误信息。

#### 没在课表上的课

要是有门课没有在课表上，我们咋办？

给 https://ehall.xidian.edu.cn/jwapp/sys/wdkb/modules/xskcb/cxxsllsywpk.do 发送 POST 请求，发送数据为：
```json
{"XNXQDM": 当前学期信息字符串}
```
然后从 `['datas']['cxxsllsywpk']` 提取信息，剩下步骤与上面一致。

#### 金智黑话翻译表

以下多数自己破译，有误请指正。

|   缩写    |                   含义                   |
| -------- | --------------------------------------- |
| KCM      | 课程名                                   |
| KCH      | 课程号                                   |
| KXH      | 教学班序号(课序号)                         |
| KCLBDM   | 课程类别                                 |
| KCXZDM   | 课程性质                                 |
| XGXKLBDM | 校公选课类别                              |
| BJDM     | 班级                                     |
| JASDM    | 上课教室                                 |
| JXLDM    | 教学楼                                   |
| JXBDM    | 教学班序号                                |
| KKDWDM   | 开课单位                                 |
| SKJS     | 授课教师                                 |
| SKXQ     | 上课星期                                 |
| XXXQDM   | 校区                                     |
| SKZC     | 上课周次(是数字数组，对应周的元素代表是否有课) |
| ZCMC     | 上课周次(字符串)                          |
| KSJC     | 开始教程                                 |
| JSJC     | 结束教程                                 |


### 获取考试安排

先说明一下，这块由于没有基本材料，我目前也没有考试，所以以下逻辑仅供参考。

#### 关于获取学期数据

我经历了那次坑爹的年初疫情，后面我们考试的时候，我们需要自己去一站式更改学期，才能看到我们拖延考试的信息。所以，这里我想做一个查看往学期考试信息的功能。

然后，当我获取所有学期代码的时候，我发现好家伙，数据居然是从 2012 年开始算的。兄弟，我 2020 年入学的好吗？

所以，我感觉每年的二月到七月算春季学期，剩下的是秋季学期，我决定自己获取学期代码。

```Dart
int now = DateTime.now().month;
String semester = "";
if (now == 1) {
    semester = "${DateTime.now().year-1}-${DateTime.now().year}-1";
} else if (now >= 2 && now <= 7) {
    semester = "${DateTime.now().year-1}-${DateTime.now().year}-2";
} else {
    semester = "${DateTime.now().year}-${DateTime.now().year+1}-1";
}
```

#### 查询考试安排信息
实际上我找到了三个相关请求，分别是：

|      缩写      |          含义           |
| ------------- | ---------------------- |
| wdksap        | 我的考试安排             |
| cxyxkwapkwdkc | 查询已选课未安排考务的课程 |
| cxwapdksrw    | 查询未安排的考试任务      |

##### 我的考试安排
给 https://ehall.xidian.edu.cn/jwapp/sys/studentWdksapApp/modules/wdksap/wdksap.do 发送 POST 请求，发送数据如下：
```json
{
    "XNXQDM":semester,
    "*order":"-KSRQ,-KSSJMS"
}
```
返回的数据在 `[datas][wdksap]` 里面，提取之。

如果有考试的话，`[extParams][code]` 为 1，数据在 `[row]` 里面。

##### 查询已选课未安排考务的课程
给 https://ehall.xidian.edu.cn/jwapp/sys/studentWdksapApp/modules/wdksap/cxyxkwapkwdkc.do 发送 POST 请求，发送数据如下：
```json
{
    "XNXQDM":semester,
}
```
返回的数据在 `[datas][cxyxkwapkwdkc]` 里面，提取之，接下来咋处理我不想说了。

##### 查询未安排的考试任务
这个我每次请求，返回的数据都是"学生查询在考试任务中且没有安排的课程"。所以我也不知道该咋办了，兴许这又是教务处啥不可说的禁区？

#### 金智黑话翻译表
打开网页调试器的网络分项，我看到了请求几个 html 的东西，里面就有缩写和对应中文，这里我摘抄几个。

|   缩写    |           含义            |
| -------- | ------------------------ |
| KSSJMS   | 考试时间                  |
| KCM      | 课程名                    |
| JASMC    | 考试地点(具体是啥我也不知道) |
| ZJJSXM   | 主讲教师                  |
| ZWH      | 座位号                    |
| YKKS     | 已完成考试                 |
| WKKS     | 未完成考试                 |
| BZ       | 考生须知                  |
| YXZXAPKS | 院系自行安排               |

### 个人信息
感觉这是最没用的功能了，除了打开应用告诉你是谁之外(溜了)

#### 获取方法
还记得获取成绩前，我们需要把跳转网址自行发送 GET 请求吗？获取个人信息时候也需要这么做:-P

如果你需要获取很基本的信息，我们往 http://ehall.xidian.edu.cn/xsfw/sys/swpubapp/userinfo/getConfigUserInfo.do?USERID=学号 发送 POST 请求。数据的 returnCode 里面，有 #E000000000000 就是成功。获取的信息在 `[data]` 里面，是一个数组。数组元素依次是学号，姓名，学院。

要想获取更加具体的信息，请往 http://ehall.xidian.edu.cn/xsfw/sys/jbxxapp/modules/infoStudent/getStuBatchInfo.do 发送信息，附带数据如下：
```json
{"requestParamStr": 你的学号}
```

#### 金智黑话翻译表
|       缩写       | 含义 |
| --------------- | ---- |
| XM              | 姓名 |
| XBDM_DISPLAY    | 性别 |
| BZ5_DISPLAY     | 书院 |
| DZ_DWDM_DISPLAY | 学院 |
| ZYDM_DISPLAY    | 专业 |
| ZSDZ            | 宿舍 |

## 总结
感觉看了那么多东西，算是把网页请求和 Cookie 啥的了解了一下，保证将来计网学的时候没有陌生感(虽然据说不咋讲)。

希望那玩意能写出来吧，免得将来找工作说没有编程经验，虽然 Dart 和 Flutter 也是很小众就是了。