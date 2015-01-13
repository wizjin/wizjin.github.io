---
layout:     post
title:      "安装配置 IntelliJ Idea"
date:       2015-01-13 19:56:14
categories: [Editor]
tags:       [Mac OSX, IntelliJ Idea]
---

自从 [`IntelliJ Idea`](https://www.jetbrains.com/idea/) 开源以后，发现越来越多被使用了，Google 的 Android Studio 也是基于 Idea 的。
之前开发 Java 使用了一段时间，发现使用上还算不错的一个 IDE，特别是开发 Java。
<!--more-->

### 安装 IntelliJ Idea

首先，在 Mac OSX 上可以[下载 Idea](https://www.jetbrains.com/idea/download/)。
其中有两个版本，一个是收费的 Ultimate Edition，另一个是免费的 Community Edition。
目前免费的版本就足够使用了。

接下来就是安装 JDK，我选择的是 1.8 版本的。可以前往 Oracle 官网[下载](http://www.oracle.com/technetwork/java/javase/downloads/index.html)。

最后，在 `~/.bash_profile` 中配置一下 JDK 的环境变量就算安装完成了。

```bash
export JAVA_HOME=$(/usr/libexec/java_home)
```

### 配置 IntelliJ Idea

由于 Idea 默认是使用 JRE 1.6 的，如果要使用 JRE 1.8 可以通过修改文件 `/Applications/IntelliJ\ IDEA\ 14\ CE.app/Contents/Info.plist`。

```xml
<key>JVMVersion</key>
<string>1.6*</string>
```

修改为：

```xml
<key>JVMVersion</key>
<string>1.8*</string>
```

接着，可以修改 Idea 的字体用，我使用免费的 [`Source Code Pro`](https://github.com/adobe-fonts/source-code-pro)。

需要修改的地方有如下几处：

- `Appearance & Behavior` / `Appearance` 中的 `Override default fonts by` 修改为 `SourceCodePro-Regular`，Size 12
- `Editor` / `Colors & Fonts` / `Fonts` 中的 `Editor Font` 修改为 `Source Code Pro`
- `Editor` / `Colors & Fonts` / `Console Font` 中的 `Editor Font` 修改为 `Source Code Pro`

这样 IntelliJ Idea 就基本上配置完成，可以使用正常使用了。
