---
layout:     post
title:      "JNI 中构造 Java 对象的注意点"
date:       2013-11-18 19:06:39
categories: [Android, C/C++]
tags:       [JNI]
---

Android NDK，在JNI中构造Java对象时有以下几点需要注意的：
<!--more-->

### 1. FindClass要在Java VM线程中调用

FindClass只能在Java VM线程中调用，所以如果要在子线程中创建Java对象的话，需要把结果保存下来，使用的方式是用NewGlobalRef创建全局引用。

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

### 2. GetFieldID中表示类型的方式与FindClass不同

FindClass中使用的是类型名称，而GetFieldID中如果使用的是类，则需要在类名前加上L，结尾加上分号。

```c
env->FindClass("com/example/myclass");
env->GetFieldID(jclz, "_mydata", "Lcom/example/myclass;");
```

### 3. AttachCurrentThread不可嵌套使用

在非Java VM线程中使用JNI需要通过AttachCurrentThread来获取JNIEnv指针，但是AttachCurrentThread函数不能递归重复使用。需要使用DetachCurrentThread释放后才能再次调用。
