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

## 进程线程概要

### 栈区是否是程序的一部分？  

不是，栈区是进程的一部分，而程序不是进程。(什么咬文嚼字233)  
概念：进程是正在运行的程序。包括程序计数器，栈区和数据区。

### 进程创建的四种情况  

系统初始化，用户请求，系统调用，批处理初始化。

### 命令行(Shell)如何执行用户指令？  

用 UNIX 系统举例 (详情见书 P88 最后一段)  

   1. 读取并解析字符串，找到程序
   2. 使用 `fork`/`exec` 系统调用生成子进程
   3. 子进程使用 `execvp` 系统调用，使用执行程序的程序段和代码段覆盖。
   4. 父进程进入等待状态。

概念：进程的状态有准备态(Ready)，等待态(Waiting)，运行态(Running)，终结态(Terminated)

### 进程终结时候发生了啥？

移除所有队列，释放占用的系统资源(内存等)，然后返回操作系统。有可能出现僵尸进程，得让父进程来终结:-P

进程终结状态如下：

   1. 程序自愿退出：执行完了/遇到一般错误
   2. 被迫退出：进程遇到严重错误/被其他进程发信号退出

## 一个上机作业

### 要求

请修改附件中的代码，实现strict alternation机制（注要能够运行）。此外需要说明程序中那个部分是关键区，以及它为什么是关键区。

### 更改后的代码

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
  while(turn!=1)    /*loop*/;
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

### 编译运行

```bash
gcc thread.c -lpthread -o thread && ./thread
ps -aux | grep thread # 记下 pid 号码
top -H -p PID # 查看该 PID 对应的线程状态
```

### 关键区

本程序是 `a` 和 `turn` ，因为它是两个进程共享的数据区域。其中 `a` 是两个进程互相访问的数据区，turn 是锁变量。
