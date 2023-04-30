+++
title = "Vala 介绍"
slug = "Vala Introduction"
description = "介绍 Vala 语言，学完了也没啥用的语言"
date = "2022-12-20"
image = "https://gitlab.gnome.org/GNOME/gnome-backgrounds/-/raw/main/backgrounds/adwaita-d.webp"
categories = [
    "Technology"
]
tags = [
    "Vala",
    "编程",
]
+++

[Vala](https://vala.dev/) 是由 [GNOME 小矮人](https://www.gnome.org/)开发的面向对象编程语言。编程语法接近 Java，围绕 [GLib](https://gitlab.gnome.org/GNOME/glib/) 库展开。编译方式是先翻译成 C 语言代码，然后编译。用途嘛......我来讲个故事吧。

我半年前学了 Dart，Google 开发的语言，编程语法接近 Javascript。官网说它是“多用途语言”，然而我感觉多数人学了它，就是为了用 Flutter :-P

Vala 也是这样，名义上是一个“多用途语言”，但是我感觉多数人学了它，只是为了 GTK。我也是不知道为啥，非得用这个语言写我的[数据库大作业](https://github.com/BenderBlog/Practise-Programs/tree/main/MySQL/exp4)，花了两周时间边学边写，最后也不知道我学了个啥......

不得不说，GLib 是一个很强大的库。本来说是给 GTK 服务的，后来独立出去了。它实现了单/双向链表，变长数组，树，Map 等数据结构。它还以 GObject 为中心，构建了一个相当完善的，庞大的，~~让我这个菜鸡不知所以的~~类系统。

接下来大致介绍顺序：

1. 基本输入输出（从键盘输入，从终端输出）

2. 判断语句 if-else 和 switch

3. 循环语句，包括计数和计事件循环

4. 我一点都不懂的面向对象

5. GLib 库和 Gee 库

6. SQLite 3 库

## 推荐链接

先给大家推荐一些前人的经验教训：

* [探索Vala语言 - 星外之神的博客 | wszqkzqk Blog](https://wszqkzqk.github.io/2022/10/17/%E6%8E%A2%E7%B4%A2Vala%E8%AF%AD%E8%A8%80/)

* [Valadoc.org (Vala 库文档网站)](https://valadoc.org/index.htm)

* [Projects/Vala/Documentation - GNOME Wiki! (官方文档)](https://wiki.gnome.org/Projects/Vala/Documentation)

## 基本输入输出

官方演示：[Projects/Vala/BasicSample - GNOME Wiki!](https://wiki.gnome.org/Projects/Vala/BasicSample)

输出一句话，都是那德行：

```vala
void main() {
    // GLib 的 print 函数
    print("Clapton is GOD!");
    // 使用到了 stdin / stdout / stderr 对象
    stdout.printf("%s is GOD!","Clapton");
}
```

输入一个数字：

```vala
void main() {
    // 双精度浮点数
    double a;
    // 类似 C 语言的 scanf，注意 out
    // 不是取地址符
    stdin.scanf("%lf",out a);
    // 类似 C 语言的 printf
    stdout.printf("%.3f",a);

    int b;
    stdin.scanf("%d",out b);
    stdout.printf("%d",b);
}
```

输入字符串：

```vala
void main() {
    stdout.printf("Welcome, what's your name?");
    string a = stdin.read_line();
    stdout.printf("OK, %s, main course is prepared for you.",a);
}
```

## 判断语句

if-else 判断：

```vala
void main() {
    stdout.printf("Enter a year: ");
    int year;
    stdin.scanf("%d",out year);
    if ((year % 4 == 0 && year % 100 != 0) || year % 400 == 0 ) {
        print("Maybe Olympics if no war.");
    } else {
        print("No Olympics.");
    }
}
```

swtich 判断：

省略，很少用到。

## 循环语句

计次数循环 for

```vala
// 金字塔输出
void main() {
    int a;
    stdin.scanf("%d",out a);
    for (int i = 1; i <= a; i += 2){
        for (int j = 0; j < (a - i) / 2; ++j) {
            print(" ");
        }
        for (int j = 0; j < i; ++j) {
            print("*");
        }
        print("\n");
    }
}
```

计事件循环 while

```vala
// Vala 引用库的方式
using Random;

void main() {
    // 这里我用了随机数类
    int toGuess = Random.int_range (0, 50);
    while (true) {
        int a;
        stdout.printf("Enter a number: ");
        stdin.scanf("%d", out a);
        if (a == toGuess) {
            break;
        }
        if (a < toGuess) {
            print("Think larger.\n");
        } else {
            print("Think smaller.\n");
        }
    }
    print("Match\n");
}
```

## 面向对象特性

先说一句，我面向对象课学的一塌糊涂，如果想了解更多，请看官方介绍：[Project/Vala/Tutorial#OOP](https://wiki.gnome.org/Projects/Vala/Tutorial#Object_Oriented_Programming)

注意，Vala 基于 GLib，GLib 包含 GObject，GObject 仅支持单向继承。所以，跟 Java 一样，Vala 是单继承+接口。

从大家喜闻乐见的开始：

```vala
class Animal {
    // 类里面的变量
    // 和 Java 一样，有 private protected public
    protected string name;
    // 构造函数
    public Animal (string _name){
        name = _name;
    }
    // 析构函数，一般不用写
    // ~Animal();
    // 方法
    public void action () {
        print("Punish you in the name of the moon, ");
    }
}

class Cat : Animal{
    private bool cute;
    public Cat (string _name, bool cute) {
        // base() 调取父类构造函数，必须写
        base(_name);
        this.cute = cute;
    }
    // 重写方法需要加 "new"
    public new void action () {
        base.action ();
        print(cute ? "meow~" : "graw~");
    }
}

void main() {
    Cat a = new Cat("A",true);
    a.action();
}
```

这个是我从网上抄的一段代码：

```vala
// 接口，也就是不能被实例化的虚类。
interface Printable {
    // abstract 要由继承的类实现
    public abstract string print ();

    // virtual 有默认的实现
    public virtual string pretty_print () {
        return "Please " + print ();
    }
}

class NormalPrint: Object, Printable {
    // 实现上面的 abstract
    string print () {
        return "don't forget about me\n";
    }
}

class OverridePrint: Object, Printable {
    string print () {
        return "Mind the gap\n";
    }

    // 重载函数，覆盖 virtual 的默认实现
    public override string pretty_print () {
        return "Override\n";
    }
}

void main (string[] args) {
    var normal = new NormalPrint ();
    var overridden = new OverridePrint ();

    print (normal.pretty_print ());
    print (overridden.pretty_print ());
}
```

## Gee

[Gee](https://wiki.gnome.org/Projects/Libgee) 相当于 C++ 里面的 STL 。我对这个了解不多，先把官方的示例贴上来：[Projects/Vala/GeeSamples - GNOME Wiki!](https://wiki.gnome.org/Projects/Vala/GeeSamples)

实际上 GLib 已经实现了很多的数据结构，但我个人建议 Gee，功能比 Glib 本身有的更丰富，但是编程的时候需要添加 Gee 库。 

```vala
using Gee;
```

Glib 中，我有用过:

* Array<类型>：变长数组

* List<类型>：双向列表

Gee中，我有用过：

* Set<类型>：无重复集合

* HashMap<类型1,类型2>：哈希字典

具体用法请参阅相关文档和示例，链接给完了，我溜了~

## 迭代，匿名函数

首先是匿名函数，很简单：

```vala
(函数形参)=>{函数体语句}
(函数形参)=>一条语句
```

一般用于函数作形参的时候，临时写一个简单的。比如下面那个情况。

还有迭代，有些预先定义好的数据结构都支持迭代，使用的时候使用 `foreach` 方法就好。比如说：

```vala
void main () {
    List<int> a = new List<int>();
    a.append (1);
    a.append (2);
    a.append (3);
    a.append (4);
    a.append (5);
    // foreach 方法需要一个函数，这里面的就是匿名函数
    a.foreach((i)=>print(i.to_string ()));
}
```

## 异常处理和空值

先写出一个错误空间，说明这是啥大类的错误，里面可以细分。

```vala
public errordomain DatabaseError {
    COULDNT_OPEN,
    EXECUTION_FAILED,
    PREPARATION_FAILED,
    BIND_FAILED,
    INVALID_GAME,
    NOT_FOUND,
}
```

写函数/方法的时候，可以加入 `throws` 关键字，注明会抛出啥错误。里面需要抛出错误的时候，使用 `throw` 语句抛出。下面是一个例子：

```vala
// 说明这个函数会抛出 DatabaseError 错误
private void open () throws DatabaseError {
    int sql_return = Sqlite.Database.open_v2 (NAME_OF_DB, out m_db);
    if (sql_return != Sqlite.OK) {
        // 这句话，先新建一个错误类，里面写的是具体内容，然后抛出
        throw new DatabaseError.COULDNT_OPEN ("Cannot create database: %d, %s\n", sql_return, m_db.errmsg ());
    }
}
```

要捕捉抛出的错误，请使用 try-catch-finally 语句：

```vala
public void createDatabase () {
    try {
        open ();
        exec (CREATE_FLIGHT_TABLE_QUERY);
        exec (CREATE_HOTEL_TABLE_QUERY);
        exec (CREATE_BUS_TABLE_QUERY);
        exec (CREATE_CUSTOMER_TABLE_QUERY);
        exec (CREATE_RESERVATION_TABLE_QUERY);
    // 错误被捕捉到了
    } catch (DatabaseError e) {
        stderr.printf ("%s\n", e.message);
    }
    // 可以加写一个 finally，finally 总会被运行
}
```

Vala 的变量可以设为空值，方法是加一个问号：

```vala
void main () {
    // 这句话会报错
    int a = null;
    // 这句话不会报错
    int ? b = null;
}
```

我个人认为，如果你不能确保方法确实能返回一个元素，可以使用这个。

```vala
int ? a () {
    try {
        int result;
        // 我没在摸鱼
        return result;    
    } catch (CatchFishBeFoundError e) {
        return null;
    }
}
```

当然，可以不用这么麻烦，这只是一个例子。

## SQLite 3 库

[SQLite](https://www.sqlite.org/index.html) 是一个库，实现了很完备的关系数据库。它将数据库存在一个文件里，使用的时候，调用 SQLite 库相应的函数，来对这个文件数据库进行基本操作。

这东西是一个 C 语言库。但 Vala 可以使用 C 库，它使用 vapi 文件来对应 C 的头文件。(实际上 Vala 也可以写 C 语言库，毕竟这玩意最后还是会变成 C 语言来编译。)

所以说，Vala 的 SQLite 库用起来应该和 C 语言的差不多。不过请注意，Vala 是面向对象的，而 SQLite 的库在引用到 Vala 的时候，做了面向对象的处理。

使用前，引用这个库：

```vala
using Sqlite;
```

### 数据库类

如此定义一个数据库对象：

```vala
Sqlite.Database m_db;
```

打开数据库：

```vala
Sqlite.Database.open_v2 (string path, out Sqlite.Database);
```

执行语句：

```vala
m_db.exec (string sql_exec);
```

### 数据库声明类

定义方式如下：

```vala
Sqlite.Statement add_flight;
```

准备声明：

```vala
public Sqlite.Statement prepare (string sql) throws DatabaseError {
    Sqlite.Statement statement;
    // 加不加 v2 都行，需要 sql 语句字符串，字符串长度，输出到一个 statement 类
    int sql_result = m_db.prepare_v2 (sql, sql.length, out statement);
    if (sql_result != Sqlite.OK) {
        throw new DatabaseError.PREPARATION_FAILED ("Cannot prepare satement for %s: %d, %s\n", sql, sql_result, m_db.errmsg ());
    }
    return statement;
}
```

绑定声明：

绑定依然有一系列的函数，此处只看绑定字符串

```vala
private void bind_text (Sqlite.Statement statement, string stmt, string text) throws DatabaseError {
    // 这是寻找 statement 中 stmt 的位置
    int index = statement.bind_parameter_index (stmt);
    if (index <= 0) {
        throw new DatabaseError.BIND_FAILED ("Could not bind %s: %s not found in the statement %s.\n", text, stmt, statement.sql ());
    }
    // 绑定，index 是索引，text 是要绑定的字符串
    int sql_result = statement.bind_text (index, text);

    if (sql_result != Sqlite.OK) {
        statement.reset ();
        throw new DatabaseError.BIND_FAILED ("Could not bind %s: %d, %s\n", text, sql_result, m_db.errmsg ());
    }
}
```

执行声明并清除绑定：

```vala
private void step (Sqlite.Statement statement) throws DatabaseError {
    // 执行声明
    int sql_return = statement.step ();
    // 清除绑定
    statement.reset ();
    if (sql_return != Sqlite.DONE) {
        throw new DatabaseError.EXECUTION_FAILED ("Execute failed: %d, %s\n", sql_return, m_db.errmsg ());
    }
    return;
}
```

循环取出返回值：

```vala
// 摘抄自我的大作业代码
public HashMap<string, HashSet<string>> ? avaliable () {
    try {
        var Graph = new HashMap<string, HashSet<string>> ();
        // 创建一个声明，这个是一个查询语句
        Sqlite.Statement get_flight = this.prepare ("SELECT FromCity,ArivCity FROM FLIGHT;");
        // 我前面说过返回值的事情，Sqlite.ROW
        while (get_flight.step () == Sqlite.ROW) {
            string from = get_flight.column_text (0);
            string to = get_flight.column_text (1);
            if (!Graph.has_key (from)) {
                Graph[from] = new HashSet<string> ();
            }
            Graph[from].add (to);
        }
        return Graph;
    } catch (DatabaseError e) {
        stdout.printf (e.message);
        return null;
    }
}
```

## 如何速通一个编程语言

我当时是这么学的 C 语言：

1. 基本输入输出

2. 判断语句

3. 循环语句

4. 函数

5. 数组

6. 结构体

7. 指针

前三条是说明这个语言大致的语法如何，因为编程思维的逻辑无非就那些：从哪里开始，需要那些材料，需要经过那些步骤，那些步骤得不断进行，这个步骤执行的条件是什么，这个步骤的结束条件是什么，最后的成果是如何的？逻辑搞明白了，接下来就是靠语言实现了。

接下来第四条，我认为是说明这个语言的性质。C 语言是面向过程的语言，所以主要是函数。而要是面向对象的话，教完函数之后，就是教你如何写一个类，如何搞继承之类的了。

剩下那三个，说明这个语言的数据结构。数据结构，有链表，栈，队列，字符串，树，图之类，还有集合，键值对字典这些常用的。这些东西给你了实现的工具，不过大多数语言已经实现了，比如 Java 。

最后，速通了语言，不代表所有。你得找到相对应的库。要是库很缺乏，或者根本没学的话，很有可能你啥都干不了。我暑假两天速通了 Javascript，然后我由于没学任何 Javascript 的库，比如 vue / react 啥的，我都不知道要用这个来干嘛:-P

最后，如有不完备或错误之处，敬请谅解。我还是水平不够啊:-(
