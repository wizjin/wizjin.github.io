---
layout:     post
title:      "Linux 下面的进程管理工具 Daemontools"
date:       2014-01-01 10:42:51
categories: [Linux]
tags:       [Linux, Tools]
---

2014年，全新的一年啊！今天想写的是 *Daemontools* 。不过这货不是哪个虚拟光驱，而是 Linux 下的进程管理工具。会用到这个是因为 Golang 写的服务程序不支持 Daemon 模式。反正理由一堆，呵呵！
<!--more-->

这次的目标很明确，就是以下几点:

- 让一个程序可以开机自动运行
- 让这个程序在挂掉后可以自动重启
- 可以查询程序状态
- 可以手工控制程序启动/停止(因为升级需要 ^0^)

然后，我选的工具就是 [Daemontools](http://cr.yp.to/daemontools/svc.html)，网上相关的文章很多，但是大多都是从源代码编译开始的。作为一个工具这样用就太麻烦了。而且我只用到了其中很小的一部分，所以研究之后直接上简单方案。

### 1. 安装Daemontools

在 Debian 下面已经有相关的 package 了，所以一条指令安装完毕:

```bash
$ apt-get install daemontools-run
```

### 2. 配置Service

接下去就是要把自己的程序变成一个服务了，方法如下:

首先，新建一个文件夹，例如 `/srv/test/` 。
路径随意，不过要有读写执行权限，因为 daemontools 会在下面生成一个 supervise 子文件夹的。

其次，在文件夹下面添加一个名为 `run` 的脚本文件，用来启动需要服务化的程序。
例如，要启用 `/root/test` 这个程序，内容可以如下:

```sh
#!/bin/sh
cd /root && /root/test
```

PS: 其实 Daemontools，并不关心这个 `run` 是什么，只要是可以执行的就可以。

### 3. 添加Service

把刚才的程序变成服务，执行下面的命令:

```bash
$ update-service --add /srv/test
```

要删除的话可以这样:

```bash
$ update-service --remove /srv/test
```

完成，不过这个时候服务不会自动启动的，要重启 Linux 才有用。需要立即启动的话，就要用到下面说的手动控制了。

### 4. 手工控制Service

查询状态

```bash
$ svstat /srv/test
```

启动服务

```bash
$ svc -u /srv/test
```

启动服务

```bash
$ svc -u /srv/test
```

其他的参数查看 help 即可，哈哈！
