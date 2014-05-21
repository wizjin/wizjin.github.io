---
layout:     post
title:      "NPAPI 中线程异步调用"
date:       2013-11-26 11:47:42
categories: [Windows, C/C++]
tags:       [NPAPI]
---

NPAPI 中有些 API 只能在主线程中调用，例如 Windowless 的 NPN_InvalidateRect 函数之类的。如果想在后台线程中刷新页面显示就需要异步调用了。在 NPAPI 中可以使用 NPN_PluginThreadAsyncCall 函数来实现。
<!--more-->

### 1. 注册函数

在 np_entry.cpp 的 NP_Initialize 函数中添加如下代码，来实现注册：

```cpp
NPNFuncs.pluginthreadasynccall = pFuncs->pluginthreadasynccall;
```

在 npn_gate.cpp 文件中添加实现代码：

```cpp
void NPN_PluginThreadAsyncCall(NPP plugin, void (*func)(void *), void *userData)
{
	NPNFuncs.pluginthreadasynccall(plugin, func, userData);
}
```

### 2. 使用 NPN_PluginThreadAsyncCall

NPN_PluginThreadAsyncCall 的参数 func 是一个回调函数，这个函数会在随后的主进程中被调用。在这个回调函数可以编写一些需要在中线程中实现的代码。
