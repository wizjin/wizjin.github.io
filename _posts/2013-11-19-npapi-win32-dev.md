---
layout:     post
title:      "Windows 下使用 NPAPI 开发浏览器插件"
date:       2013-11-19 09:43:06
categories: [Windows, C/C++]
keywords:   [NPAPI]
---

最近需要开发一个浏览器的插件。浏览器的插件在Windows下面被分为了两类，一类是利用IE内核的，其插件是ActiveX的模式；另一类是遵循NPAPI的模式。ActiveX的可以选择在VC++中利用ATL来编写，NPAPI的模式以Firefox为例，也可以通过VC++来开发。
<!--more-->

### 1. 获取NPAPI SDK

NPAPI的SDK有两种方式获取：

- Google code上托管的[npapi-sdk](https://code.google.com/p/npapi-sdk/)
- [firefox4.0.1源代码](http://ftp.mozilla.org/pub/mozilla.org/firefox/releases/4.0.1/source/firefox-4.0.1.source.tar.bz2)解压后，其目录 `.\mozilla-2.0\modules\plugin` 中包含的SDK文件

### 2. 创建工程

- 在Visual Studio中创建一个动态链接库Dll工程。
- 添加SDK中的头文件和np_entry.cpp、npn_gate.cpp和npp_gate.cpp三个文件
- 添加预编译宏_X86_

### 3. 配置Dll导出函数

添加一个def定义文件，用来确保Dll导出函数正确，内容如下：

```
LIBRARY "npdemo"
EXPORTS
	NP_GetEntryPoints	@1
	NP_Initialize		@2
	NP_Shutdown		@3
```

### 4. 配置VERSION资源

添加一个rc资源文件，并添加一条VERSION资源，修改StringFileInfo块

- 将其中的子BLOCK设置成 @040904e4@
- 添加一条 `VALUE "MIMEType", "application/demo-plugin"`，NPAPI是利用MIMEType来查找插件的，所以这里的demo-plugin其实就是插件的标识。当然，如果写成常用的类型，则表示这个插件可以支持打开这种类型的资源

### 5. 插件实现代码

向工程添加一个C++类，例如Plugin。代码如下：

```cpp
// Plugin.h
#include "pluginbase.h"
class CPlugin : public nsPluginInstanceBase
{
public:
	CPlugin(NPP pNPInstance);
	~CPlugin();
	NPBool init(NPWindow* pNPWindow) {
		m_bInitialized = TRUE;
		return TRUE;
	}
	void shut() {
		m_bInitialized = FALSE;
	}
	NPBool isInitialized() {
		return m_bInitialized;
	}
private:
	NPP m_pNPInstance;
	NPBool m_bInitialized;
};
```

```cpp
// Plugin.cpp
#include "plugin.h"
////// functions /////////
NPError NS_PluginInitialize()
{
	return NPERR_NO_ERROR;
}
void NS_PluginShutdown()
{
}
nsPluginInstanceBase * NS_NewPluginInstance(nsPluginCreateData *aCreateDataStruct)
{
	if(!aCreateDataStruct)
		return NULL;
	CPlugin * plugin = new CPlugin(aCreateDataStruct->instance);
		return plugin;
}
void NS_DestroyPluginInstance(nsPluginInstanceBase * aPlugin)
{
	if(aPlugin)
		delete (CPlugin *)aPlugin;
}
////// CPlugin /////////
CPlugin::CPlugin(NPP pNPInstance)
: nsPluginInstanceBase(),
  m_pNPInstance(pNPInstance),
  m_bInitialized(FALSE)
{
}
CPlugin::~CPlugin()
{
}
```

### 6. 安装插件

Firefox插件通过注册表安装，在注册表 `HKEY_CURRENT_USER\SOFTWARE\MozillaPlugins` 下面，新建子项 @mozilla.com.cn/demo，并新建字符串数据"Path"设值Dll完整路径。

### 7. 测试插件

可以编写一个html测试页面来测试插件：

```html
<HTML>
	<HEAD></HEAD>
	<BODY>
		<embed type="application/demo-plugin">
	</BODY>
</HTML>
```

PS. 在Firefox中利用 `about:plugins` 可以查看插件的状况。
