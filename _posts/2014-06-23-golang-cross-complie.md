---
layout:     post
title:      "Golang 交叉编译"
date:       2014-06-23 13:03:07
categories: [Golang]
tags:       [Linux, Mac OSX]
---

由于最近项目需要，有一些临时性的微服务打算用 Golang 来开发。
而我的电脑是 iMac，服务器是 Linux。所以需要使用交叉编译。
<!--more-->

### 编译 Golang 环境

以 Mac OSX 为例，首先需要进入 Go 的源码目录，然后编译环境。
可以通过下列命令实现

```bash
$ cd /usr/local/go/src
$ CGO_ENABLED=0 GOOS=linux GOARCH=amd64 ./make.bash
$ CGO_ENABLED=0 GOOS=windows GOARCH=amd64 ./make.bash
```

这里可以按照需要选择编译 Linux/windows 的环境。

### 交叉编译

现在可以使用交叉编译了，编译的命令如下:

```bash
$ CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build 
$ CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build
```

PS. 这种方式暂时不支持 CGO
