# 操作系统：严格轮换机制

## 要求
请修改附件中的代码，实现strict alternation机制（注要能够运行）。此外需要说明程序中那个部分是关键区，以及它为什么是关键区。


## 更改后的代码
```C
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

// Public Area.
// Critical region, because both threads needs to access this.
int a;

// Thread 2.
void * th(void *p)
{
	while(1)
	{
		//a=1;
		while(a!=1) /*loop*/;
		sleep(1);
		printf("a=%d haha\n",a);
		a=0;
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
		while(a!=0);
		sleep(1);
		printf("a=%d hehe\n",a);
		a=1;
	}
}
```

## 关键区
本程序是 `a` ，因为它被两个线程轮流访问。