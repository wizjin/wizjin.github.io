---
layout:     post
title:      "NPAPI 中线程异步调用"
date:       2013-11-26 11:47:42
categories: [Windows, C/C++]
keywords:   [NPAPI]
---

NPAPI中有些API只能在主线程中调用，例如Windowless的NPN_InvalidateRect函数之类的。如果想在后台线程中刷新页面显示就需要异步调用了。在NPAPI中可以使用NPN_PluginThreadAsyncCall函数来实现。
<!--more-->

### 1. 注册函数

在np_entry.cpp的NP_Initialize函数中添加如下代码，来实现注册：

```cpp
NPNFuncs.pluginthreadasynccall = pFuncs->pluginthreadasynccall;
```

在npn_gate.cpp文件中添加实现代码：

```cpp
void NPN_PluginThreadAsyncCall(NPP plugin, void (*func)(void *), void *userData)
{
	NPNFuncs.pluginthreadasynccall(plugin, func, userData);
}
```

### 2. 使用NPN_PluginThreadAsyncCall

NPN_PluginThreadAsyncCall的参数func是一个回调函数，这个函数会在随后的主进程中被调用。在这个回调函数可以编写一些需要在中线程中实现的代码。
