---
layout:     post
title:      "Android 加载动态连接库"
date:       2013-10-24 10:21:59
categories: [Android, C/C++]
keywords:   [Android]
---

Android加载动态连接库(.so文件)有以下两种方法<!--more-->：

- 在Java中使用System.loadLibrary函数
- 在NDK中使用dlopen/dlsym/dlclose函数

### System.loadLibrary实现方式

为了能在App启动进入Activity前把动态连接库加载好，一般会选择把loadLibrary放在static中，写法如下：

```java 
public class MyClass {
	static {
		System.loadLibrary("mylib.so");
	}
	... ...
};
```

### dlopen/dlsym/dlclose实现方式

工作原理基本上是，dlopen用来加载动态连接库，dlsym从dlopen加载的动态链接库中按函数名获取函数指针。

例如，mylib.so中有一个add函数：

```java
int add(int a, int b) {
	return a+b;
}
```

需要加载这个函数就可以这样来写：

```java
static char libpath[MAX_PATH];
typedef int (*func_add)(int, int);
jint JNI_OnLoad(JavaVM* vm, void* reserved) {
	void* lib = dlopen(libpath, RTLD_NOW);
	if (lib) {
		func_add add = (func_add) dlsym(lib, "add");
		if (add) add(1, 2);
		dlclose(lib);
	}
	return JNI_VERSION_1_6;
}
```

这里遇到一个关键就是dlopen的第一个参数需要指明动态连接库的路径，这个路径怎么确定呢？如果知道这个路径的话直接写就可以了。当然，这里有个方法可以根据函数获得所在动态连接库路径。方法如下：

```c
Dl_info info;
bzero(&info, sizeof(info));
if (dladdr((void*)&JNI_OnLoad, &info) != 0) {
	char lineBuf[2048];
	FILE* fp = fopen("/proc/self/smaps", "rt");
	if (fp) {
		char addrBuf[18];
		sprintf(addrBuf, "%p-", info.dli_fbase);
		int addrLength = strlen(addrBuf);
		while (fgets(lineBuf, sizeof lineBuf, fp) != NULL) {
			if(strncmp(lineBuf, addrBuf, addrLength) == 0
				|| strncmp(lineBuf, addrBuf+2, addrLength-2) == 0) {
				// .so文件记录
				int lineLength = strlen(lineBuf);
				int libNameLength = strlen(info.dli_fname);
				for(int i = lineLength - 1; i >= 0 && isspace(lineBuf[i]); --i) {
					lineBuf[i] = 0;
					--lineLength;
				}
				if (strncmp(lineBuf + lineLength - libNameLength,
					info.dli_fname, libNameLength) != 0) {
					continue;
				}
				char* pathBegin = strchr(lineBuf, '/');
				if (pathBegin == 0) continue;
				char* pathEnd = strrchr(pathBegin, '/');
				pathEnd[1] = 0;
				// pathBegin此时指向的就是so文件所在目录的路径了
				break;
			}
		}
		fclose(fp);
	}
}
```

用这个方式加载自由度会高很多，至少我这么认为的！哈哈！:)
