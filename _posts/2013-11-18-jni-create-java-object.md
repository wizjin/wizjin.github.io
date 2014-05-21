---
layout:     post
title:      "JNI 中构造 Java 对象的注意点"
date:       2013-11-18 19:06:39
categories: [Android, C/C++]
tags:       [JNI]
---

Android NDK，在 JNI 中构造 Java 对象时有以下几点需要注意的：
<!--more-->

### 1. FindClass 要在 Java VM 线程中调用

FindClass 只能在 Java VM 线程中调用，所以如果要在子线程中创建 Java 对象的话，需要把结果保存下来，使用的方式是用 NewGlobalRef 创建全局引用。

```c
jclass javaClassLocal	= env->FindClass(kAndroidJavaClass);
if (javaClassLocal) {
	m_javaClassLocal = (jclass)env->NewGlobalRef(javaClassLocal);
	env->DeleteLocalRef(javaClassLocal);
}
```

全局引用要在程序退出前销毁。

```c
if (m_javaClassLocal) env->DeleteGlobalRef(m_javaClassLocal);
```

### 2. GetFieldID 中表示类型的方式与 FindClass 不同

FindClass 中使用的是类型名称，而 GetFieldID 中如果使用的是类，则需要在类名前加上 L，结尾加上分号。

```c
env->FindClass("com/example/myclass");
env->GetFieldID(jclz, "_mydata", "Lcom/example/myclass;");
```

### 3. AttachCurrentThread 不可嵌套使用

在非 Java VM 线程中使用 JNI 需要通过 AttachCurrentThread 来获取 JNIEnv 指针，但是 AttachCurrentThread 函数不能递归重复使用。需要使用 DetachCurrentThread 释放后才能再次调用。
