---
layout:     post
title:      "Debian 上安装 Erlang-OTP"
date:       2013-11-15 08:38:50
categories: [Linux, Erlang]
tags:       [Erlang, Debian]
---

要在 Debian 上安装 Erlang-OTP 现在变得十分简单了，可以直接下载 deb 包安装，也可以通过源来安装。
<!--more-->

### 1. 直接下载deb包安装

前往 erlang-solutions 的[下载页面](https://www.erlang-solutions.com/downloads/download-erlang-otp)，下载对应的 deb 包安装就可以了。其中有 32-bit 和 64-bit，支持 wheezy 和 squeeze。

### 2. 通过源来安装

- 在 `/etc/apt/sources.list` 文件中添加源地址

```
deb http://packages.erlang-solutions.com/debian wheezy contrib
deb http://packages.erlang-solutions.com/debian squeeze contrib
```

- 添加 erlang-solutions 的 key

```bash
$ wget http://packages.erlang-solutions.com/debian/erlang_solutions.asc
$ sudo apt-key add erlang_solutions.asc
```

- 安装 Erlang

```bash
$ sudo apt-get update
$ sudo apt-get install erlang
```

### 3. 安装 HiPE

如果需要安装 HiPE，则使用下面的命令:

```bash
$ sudo apt-get install erlang-base-hipe
```

如果要还原成不带 HiPE，则使用下面的命令:

```bash
$ sudo apt-get install erlang-base
```
