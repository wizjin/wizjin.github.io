---
layout:     post
title:      "保存 Linux 程序 Crash 信息"
date:       2013-11-09 11:55:28
categories: [Linux, C/C++]
tags:       [Linux, Crash]
---

在Linux下面编写程序，遇到Crash，特别是服务程序时，将会是一件让人头疼的事。要查找问题，只能查看log，但是这常常是一个痛苦的过程。如果可以吧Crash时候的调用堆栈保存下来，这将有着莫大的帮助。
<!--more-->

### 输出堆栈示例代码

```c
#include <signal.h>
#include <execinfo.h>
// 输出堆栈信息
static void backtrace_handler(int signum)
{
	void* array[10];
	size_t size;
	char** strings;
	signal(signum, SIG_DFL); // 还原默认信号处理
	size = backtrace(array, sizeof(array)/sizeof(array[0]));
	strings = (char **)backtrace_symbols(array, size);
	printf("Stack trace:\n");
	for (size_t i = 0; i < size; i++) {
		printf("%d %s\n", i, strings[i]);
	}
	free(strings);
	exit(1);
}
// 设置输出函数
int main(void)
{
	// ...
	signal(SIGSEGV, backtrace_handle);
	signal(SIGABRT, backtrace_handle);
	// Do something
}
```

### 使用注意事项

- gcc编译时需要添加参数-g
- gcc编译时如果使用-g -rdynamic参数，则可以加强出错信息的细节
- 在获得出错地址时，例如0x80123450，则在gdb中可使用以下两中方式查看:
	+ info line *0x80123450
	+ list *0x80123450
