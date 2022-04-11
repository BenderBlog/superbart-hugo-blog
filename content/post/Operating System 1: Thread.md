+++
title = "操作系统：线程"
slug = "Operating System 1: Thread"
description = "仅供参考，我不知道对不对"
date = "2022-04-09"
image = "https://legacy.superbart.xyz/picture/Random/Gunner%20from%20Quake%20II%20-%20modified.png"
categories = [
    "Technology"
]
tags = [
    "操作系统"
]

+++

## 进程概要

### 栈区是否是程序的一部分？  

不是，栈区是进程的一部分，而程序不是进程。(什么咬文嚼字233)  
概念：进程是正在运行的程序。包括程序计数器，栈区和数据区。

### 进程创建的四种情况是啥？

系统初始化，用户请求，系统调用，批处理初始化。

## 父子进程

###  Fork (创建子进程) 和 Exec (执行) 的区别

我觉得括号里面说的很明白了2333  

 *  Fork: 子进程和父进程的代码区，堆栈区是相同的。
 *  Exec: 在同一个进程中，用程序镜像替换这个进程（使用执行程序的程序段和代码段覆盖）。

### 命令行(Shell)如何执行用户指令？  

用 UNIX 系统举例 (详情见书 P88 最后一段)  

   1. 读取并解析字符串，找到程序
   2. 使用 `fork`/`exec` 系统调用生成子进程
   3. 子进程使用 `execvp` 系统调用，使用执行程序的程序段和代码段覆盖。
   4. 父进程进入等待状态。

概念：进程的状态有准备态(Ready)，等待态(Waiting)，运行态(Running)

### 读这段代码，说最终有几个进程捏？

先告诉你有四个。  

```C
int main(){
	pid_t pid;
	for (int i = 0; i < 2; i++)
		pid = fork();
}
```

![Let's paint a tree, shall we?](https://legacy.superbart.xyz/picture/Operating%20System%201:%20Thread/PROCESS_FORK_TREE.jpg)

## 深入进程

### 进程终结时候发生了啥？

移除所有队列，释放占用的系统资源(内存等)，然后返回操作系统。有可能出现僵尸进程，得让父进程来终结:-P

进程终结状态如下：

   1. 程序自愿退出：执行完了/遇到一般错误
   2. 被迫退出：进程遇到严重错误/被其他进程发信号退出

### PCB 不是电路板！

PCB (Process Control Block)：进程控制块。重点包括以下东西：

1. 进程状态
2. 程序计数器
3. CPU寄存器
4. CPU调度信息
5. 内存管理信息
6. Accounting information
7. 输入输出状态信息

注意：第六条直译为会计信息。我有两个理解：进程状态信息 / 用户信息
（我的上帝啊，谁能给我本翻译得十分不错的 Modern Operating System 啊）

## 线程概要

### 线程提出的目的

对于操作系统来说，中断或者切换一个进程的代价太高了。

### 弹出式线程的定义

操作系统收到一个信息后，创建一个线程来处理信息。  
概念：进程分为用户态线程（管理归进程），系统态线程（管理归操作系统），混合态进程，以及弹出态进程

### 用户态线程的优劣

#### 优点

1. 线程调用很快。
2. 线程可以自行定义调度算法。
3. 减轻内核调用线程压力。（内核看不到用户态线程）

#### 缺点

1. 线程无法调用阻塞式系统调用。（毕竟只能访问进程里面的玩意）
2. 没有时钟，除非运行完退出，其他线程无法运行。

## 调度算法

### 调度发生的时机

1. 新进程的创建
2. 进程的退出
3. 某进程需要IO操作,
4. IO设备申请CPU中断 (称之为IO中断)

### 一道计算题

![](https://legacy.superbart.xyz/picture/Operating%20System%201:%20Thread/SCHEDULER_Q1-1.png)
![](https://legacy.superbart.xyz/picture/Operating%20System%201:%20Thread/SCHEDULER_Q1-2.png)
![](https://legacy.superbart.xyz/picture/Operating%20System%201:%20Thread/SCHEDULER_Q1-3.png)
![](https://legacy.superbart.xyz/picture/Operating%20System%201:%20Thread/SCHEDULER_Q1-A.png)

## 进程间通信

### 基础概念

* 竞争条件 (race condition) ：多个进程间读取一个数据。
* 临界区 (critical region)：要与其他进程共享数据区域。
* 互斥访问 (mutual exclusion) 和忙等待 (busy waiting)

###  互斥访问方案原则

1. 任意两个进程不能同时在临界区。
2. 不对 CPU 速度和数量做出假设。
3. 临界区外运行进程不能阻塞其他进程。
4. 不要让进程进入临界区前无限制等待。

### 阅读代码，填空

```asm
enter_region:
	MOVE REGISTER,___(1)___;
	XCHG REGISTER,LOCK;
	CMP  REGISTER,#1;
	JE   OK;
	CALL ___(2)___;
	JMP  ___(3)___;
ok: RET;

leave_region:
	MOVE LOCK,___(4)___;
	RET;
```

根据 `CMP REGISTER,#1; JE OK; ok: RET;` 可分析出 `#1` 是没上锁，`#0` 是上锁了。  
`XCHG` 可以互换两个寄存器的值，如果第一个空填入的是 `#1` ，那么无论如何，判断都是没上锁。所以第一个空填入 `#0` 。  
第二个和第三个空是忙等待的东西，分别填的是 `thread_yield` （找另外一个进程）和 `enter_region` 。  
第四个空填入 `#1` ，用完了就解锁。

概念 互斥访问策略

1. 关闭中断。(单 CPU 有效)
2. 锁变量与忙等待。
3. 严格轮换。
4. Peterson 算法。
5. 汇编 `TSL` 指令。

### 严格轮换机制作业

请修改附件中的代码，实现strict alternation机制（注要能够运行）。此外需要说明程序中那个部分是关键区，以及它为什么是关键区。

```C
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h> // For sleep().

// Critical region, because both threads needs to access this.
int a;
// Lock Variable
int turn = 0;

// Thread 2.
void * th(void *p)
{
	while(1)
	{ 
		while(turn!=1) /*loop*/;
		sleep(1);a=0;   // Access Critical Region
		printf("a=%d haha\n",a);// Stop access.
		turn=0;     // Change mark.
 	}
}

int main()
{
     a=0;
     pthread_t myth;
     pthread_create(&myth,NULL,th,NULL);

     // Thread 1.
     while(1)
     {
		while(turn!=0)   /*loop*/;
		sleep(1);a=1;   // Access Critical Region.
		printf("a=%d hehe\n",a);// Stop access.
		turn=1;     // Change mark.
     }
}

```

以下是编译运行方式。

```bash
gcc thread.c -lpthread -o thread && ./thread
ps -aux | grep thread # 记下 pid 号码
top -H -p PID # 查看该 PID 对应的线程状态
```

本程序的关键区是 `a` 和 `turn` ，因为它是两个进程共享的数据区域。其中 `a` 是两个进程互相访问的数据区，turn 是锁变量。

### 生产者和消费者问题

阅读代码，看看有啥问题。

![](https://legacy.superbart.xyz/picture/Operating%20System%201:%20Thread/IPC_Q1.png)

在单核 CPU 上：  

先执行消费者进程：  
发现仓库里没有东西，准备睡眠。但是还没睡眠，进程切换到生产者了。  
生产者开始生产产品，发现仓库里有东西，向消费者进程发出唤醒信号。进程切换到消费者。  
消费者进程是醒着的，只是准备睡眠，把唤醒信号忽略掉，然后睡眠了，退出了 CPU 。  
最后生产者把仓库填满了，也睡了。两个进程都睡了:-P 

如果目前仓库满了：  
生产者决定睡眠，但还没睡，进程切换到消费者了。   
消费者用了一个产品，发现仓库里数量为 N-1，唤醒生产者。  
生产者忽略了唤醒信号，睡眠。  
消费者用完了所有东西，也睡了:-P

要点：准备睡眠时切换进程了，唤醒信号被忽略。

### Mutex（互斥锁）和 Semaphore（信号量）的不同

Mutex 实现在用户态，代价低，数量上限高。Semaphore 实现在内核态，代价高，数量上限低。
