---
layout:     post
title:      "NPAPI 中添加 Javascript 函数"
date:       2013-11-21 10:20:23
categories: [Windows, C/C++]
tags:       [NPAPI, Scriptable]
---

NPAPI 中，如果一个插件需要向 Javascript 开放接口函数，那么其实是一件比较复杂的工作。因为其实是需要通过返回一个脚本对象来实现。
<!--more-->

### 1. 脚本对象入口

整个脚本对象的入口其实是在 NPP_GetValue 函数中的，具体的方式如下：

```cpp
NPError NPP_GetValue(NPP instance, NPPVariable variable, void *value)
{
	// ...
	switch (variable) {
	// ...
	// https://developer.mozilla.org/en/Gecko_Plugin_API_Reference/Scripting_plugins
	case NPPVpluginScriptableNPObject:
		*(NPObject **)value = plugin->GetScriptableObject();
		break;
	// ...
	}
	// ...
}
```

GetScriptableObject 就是在实现 Plugin 时需要返回的一个 NPObject 对象

### 2. ScriptablePluginObject 对象

ScriptablePluginObject 对象是继承自 NPObject 的，在 NPAPI 的 SDK 中有例子，来说明如何使其工作的，其中的主要部分被封装在了 ScriptablePluginObjectBase 对象中。只需要集成 ScriptablePluginObjectBase，实现接口即可。

```cpp
class ScriptablePluginObject : public ScriptablePluginObjectBase
{
public:
	ScriptablePluginObject(NPP npp) : ScriptablePluginObjectBase(npp){}
	virtual bool HasMethod(NPIdentifier name);
	virtual bool HasProperty(NPIdentifier name);
	virtual bool GetProperty(NPIdentifier name, NPVariant *result);
	virtual bool Invoke(NPIdentifier name, const NPVariant *args, uint32_t argCount, NPVariant *result);
	virtual bool InvokeDefault(const NPVariant *args, uint32_t argCount, NPVariant *result);
};
```

Javascript 调用函数的时候，首先会通过函数 HasMethod 询问调用的方法是否存在，如果存在返回 true，之后便会调用 Invoke 函数来执行实际的运行代码。

### 3. windowless控件

在页面上显示的控件有 window 和 windowless 两种模式，我更倾向于使用 windowless。纯属个人喜好！呵呵！实现 windowless 模式需要以下几步：

- 在 NPP_New 函数创建完插件后调用 NPN_SetValue，激活 windowless 模式。

```cpp
NPError NPP_New(NPMIMEType pluginType, NPP instance, uint16_t mode,
				int16_t argc, char* argn[], char* argv[],
				NPSavedData* saved)
{
	// ...
	NPN_SetValue(instance, NPPVpluginWindowBool, NULL);
	// ...
}
```

- 在 NPP_SetWindow 中设置 NPWindow

```cpp
NPError NPP_SetWindow (NPP instance, NPWindow* pNPWindow)
{
	// ...
	// window goes away
	if((pNPWindow->window == NULL) && pPlugin->isInitialized())
		return pPlugin->SetWindow(pNPWindow);
	// window resized
	if(pPlugin->isInitialized() && (pNPWindow->window != NULL))
		return pPlugin->SetWindow(pNPWindow);
	// this should not happen, nothing to do
	if((pNPWindow->window == NULL) && !pPlugin->isInitialized())
		return pPlugin->SetWindow(pNPWindow);
	return rv;
}
```

- 使用 NPP_HandleEvent 来处理消息

```cpp
int16_t NPP_HandleEvent(NPP instance, void* event)
{
	NPEvent * event = (NPEvent *)event;
	switch (event->event) {
	case WM_PAINT:
		// ...
		{
			RECT * drc = (RECT *)event->lParam;
			HDC hDC = (HDC)event->wParam;
			// ...
		}
		break;
	default:
		break;
	}
	return 1;
}
```

如此之后，就可以开放接口给 Javascript 来调用插件的函数，实现更加复杂的功能了。
