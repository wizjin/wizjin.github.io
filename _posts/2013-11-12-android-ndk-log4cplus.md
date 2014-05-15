---
layout:     post
title:      "Android NDK 编译 Log4cplus 1.1.1"
date:       2013-11-12 10:20:21
categories: [Android, C/C++]
keywords:   [Android NDK, log4cplus]
---

编写Android NDK的程序时候调试起来很不方便，只能依靠__android_log_print来输出Log定位问题。但是不幸的是最近发现有的手机上用logcat来保存log有问题。所以，只能寻找别的解决方案，然后就是发现了Log4cplus，而且其还带有android的编译脚本。
<!--more-->

首先，下载[log4cplus](http://sourceforge.net/p/log4cplus/wiki/Home/)，并且解压。其中可以发现有android目录，里面是用cmake来编译的。Windows下需要的NDK版本是r7以上。还是挺方便的，不过有几个地方需要修改。

### 1. 修改src目录下面的CMakeLists.txt文件，去除VERSION和SOVERSION两项。

```
set_target_properties (${log4cplus} PROPERTIES
	OUTPUT_NAME log4cplus
	COMPILE_FLAGS "-DINSIDE_LOG4CPLUS")
```

> 由于默认生成的是liblog4cplus.so、liblog4cplus.so.7和liblog4cplus.so.1.1.1三个文件，但是其实连接的是liblog4cplus.so.7这个库。并且由于Android NDK的Makefile使用非so结尾的共享库会报错，暂时没有找到解决方案。所以这里直接强制只生成liblog4cplus.so文件。

### 2. 编译log4cplus

```shell
$ SET ANDROID_NDK=C:\absolute\path\to\the\android-ndk
$ mkdir build && cd build
$ cmake.exe -G"MinGW Makefiles"
	-DANDROID_CXX_FLAGS="-std=c++11"
	-DANDROID_NATIVE_API_LEVEL=android-9
	-DCMAKE_TOOLCHAIN_FILE=path\to\the\android.toolchain.cmake
	-DCMAKE_MAKE_PROGRAM="%ANDROID_NDK%\prebuilt\windows\bin\make.exe" ..
$ cmake.exe --build .
```

> -  利用说明文件中的方式编译，会带有pthread_rwlock_destroy错误，主要是应为pthread的读写锁在android-9之后才开始支持。所以要添加 `-DANDROID_NATIVE_API_LEVEL=android-92`，不然编译不过。
- 添加 `-DANDROID_CXX_FLAGS="-std=c++11"`，并且使用的时候要在Apllication.mk文件中添加 `APP_CPPFLAGS := std=c++11`。因为log4cplus中使用了部分C++11的新语法。
