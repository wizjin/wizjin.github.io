---
layout:     post
title:      "Android NDK 编译 Log4cplus 1.1.1"
date:       2013-11-12 10:20:21
categories: [Android, C/C++]
tags:       [Android NDK, log4cplus]
---

编写 Android NDK 的程序时候调试起来很不方便，只能依靠 __android_log_print 来输出 Log 定位问题。但是不幸的是最近发现有的手机上用 logcat 来保存 log 有问题。所以，只能寻找别的解决方案，然后就是发现了 Log4cplus，而且其还带有 Android 的编译脚本。
<!--more-->

首先，下载[log4cplus](http://sourceforge.net/p/log4cplus/wiki/Home/)，并且解压。其中可以发现有 android 目录，里面是用 cmake 来编译的。Windows 下需要的 NDK 版本是 r7 以上。还是挺方便的，不过有几个地方需要修改。

### 1. 修改 src 目录下面的 CMakeLists.txt 文件，去除 VERSION 和 SOVERSION 两项。

```
set_target_properties (${log4cplus} PROPERTIES
	OUTPUT_NAME log4cplus
	COMPILE_FLAGS "-DINSIDE_LOG4CPLUS")
```

> 由于默认生成的是 liblog4cplus.so、liblog4cplus.so.7 和 liblog4cplus.so.1.1.1 三个文件，但是其实连接的是 liblog4cplus.so.7 这个库。并且由于 Android NDK 的 Makefile 使用非 so 结尾的共享库会报错，暂时没有找到解决方案。所以这里直接强制只生成 liblog4cplus.so 文件。

### 2. 编译 log4cplus

```bash
$ SET ANDROID_NDK=C:\absolute\path\to\the\android-ndk
$ mkdir build && cd build
$ cmake.exe -G"MinGW Makefiles"
	-DANDROID_CXX_FLAGS="-std=c++11"
	-DANDROID_NATIVE_API_LEVEL=android-9
	-DCMAKE_TOOLCHAIN_FILE=path\to\the\android.toolchain.cmake
	-DCMAKE_MAKE_PROGRAM="%ANDROID_NDK%\prebuilt\windows\bin\make.exe" ..
$ cmake.exe --build .
```

> -  利用说明文件中的方式编译，会带有 pthread_rwlock_destroy 错误，主要是应为 pthread 的读写锁在 android-9 之后才开始支持。所以要添加 `-DANDROID_NATIVE_API_LEVEL=android-92`，不然编译不过。
- 添加 `-DANDROID_CXX_FLAGS="-std=c++11"`，并且使用的时候要在 Apllication.mk 文件中添加 `APP_CPPFLAGS := std=c++11`。因为 log4cplus 中使用了部分 C++11 的新语法。
