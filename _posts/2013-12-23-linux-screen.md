---
layout:     post
title:      "Linux 使用 screen 管理远程会话"
date:       2013-12-23 10:46:54
categories: [Linux]
keywords:   [Linux, Screen]
---

有时候需要远程登录到liunx服务器，当要运行一些耗时的任务时候就会变得麻烦。因为一旦退出登录，运行的程序就会被杀掉。万一网络不好SSH或者Telnet断了就更加悲剧了。幸好linux下面有screen命令可以解决这个问题。:)
<!--more-->

### 1. 直接使用 Screen

```shell
$ screen
```

建立一个匿名的 screen，通过 exit 退出。感觉不太长用。

### 2. Screen+ 执行的命令

``shell
$ screen vim readme
```

创建一个 screen 用于执行 vim 命令，退出 vim 将会关闭该这个 screen。

### 3. 断开当前screen

只需要输入 `Ctrl-a d` 就可以了。

### 4. 重新连接会话

首先利用 `screen -ls` 查看已近存在的会话，然后通过 `screen -r <screen id>` 来重新连接会话。

PS. 其他的命令可以通过 `Ctrl-a ?` 来查询，简单方便。

### 5. 常用的命令

其实最常用的方法是下面这2个：

- 创建指定名字的screen - `$ screen -dmS <name>`
- 连接指定名字的screen - `$ screen -r <name>`
