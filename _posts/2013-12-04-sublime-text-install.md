---
layout:     post
title:      "Sublime Text 3 安装配置"
date:       2013-12-04 21:46:23
categories: [Editor]
tags:       [Sublime]
---

最近在 Windows 下面想找一款轻量级的编辑器，发现 [Sublime Text 3](http://www.sublimetext.com/3) 不错，就装了试试看。
<!--more-->

### 1. 安装 Sublime Text 3

下载 Sublime Text 3 对应平台的安装包，来安装。

### 2. 安装 Package Control

安装 [Package Control](https://sublime.wbond.net/installation)，通过 __ctrl+\`__ 执行下面的代码即可。

```python
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

重启 Sublime Text 后 Package Control 就能生效了。

### 3. 安装插件

- Theme – Soda 主题
- Alignment 等号对齐(Ctrl+Alt+A)
- SideBarEnhancements 侧边栏增强
- GoSublime 支持 Go 语言
- Git git 工具

### 4. 支持 VIM

默认 Sublime Text 是关闭对 Vim 的支持的，在 setting 里面去除 `"ignored_packages": ["vintage"]`，就可以支持 VIM。配置文件如下:

```json
{
	"gutter": false, 		//是否显示边列
	"wrap_width": 80, 	//换行宽度(单位：字符)
	"word_wrap": true,	//是否自动换行
	"scroll_past_end": true,	//滚动能否超过结尾
	"ignored_packages":[],	// 支持VIM
	"tab_size": 4,	// 设置Tab宽度
	"theme": "Soda Dark.sublime-theme"	// 设置主题
}
```

### 4. 支持 Go 语言

安装 GoSublime，然后修改其 `Settings - User` 配置文件

```json
{
    "autocomplete_builtins": true,
    "autocomplete_suggest_imports": true
}
```

### 5. 配置工程文件

添加工程文件，并将 build system 选择为 GoSublime。
