+++
title = "操作系统：严格轮换机制作业"
slug = "Operating System 1: Strict Alternation"
description = "仅供参考，我不知道对不对"
date = "2022-03-01"
image = "https://legacy.superbart.xyz/picture/Random/Gunner%20from%20Quake%20II%20-%20modified.png"
categories = [
    "Technology"
]
tags = [
    "操作系统"
]
+++


# 操作系统：严格轮换机制作业

## 要求
请修改附件中的代码，实现strict alternation机制（注要能够运行）。此外需要说明程序中那个部分是关键区，以及它为什么是关键区。


## 更改后的代码
```C
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>	// For sleep().

// Critical region, because both threads needs to access this.
int a;
// Lock Variable
int turn = 0;

// Thread 2.
void * th(void *p)
{
	while(1)
	{	
		while(turn!=1) 			/*loop*/;
		sleep(1);a=0;			// Access Critical Region
		printf("a=%d haha\n",a);// Stop access.
		turn=0;					// Change mark.
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
		while(turn!=0)			/*loop*/;
		sleep(1);a=1;			// Access Critical Region.
		printf("a=%d hehe\n",a);// Stop access.
		turn=1;					// Change mark.
		
	}
}

```
## 编译运行
```bash
$ gcc thread.c -lpthread -o thread && ./thread
$ ps -aux | grep thread # 记下 pid 号码
$ top -H -p PID # 查看该 PID 对应的线程状态
```

## 关键区
本程序是 `a` 和 `turn` ，因为它是两个进程共享的数据区域。其中 `a` 是两个进程互相访问的数据区，turn 是锁变量。