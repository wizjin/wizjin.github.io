---
layout:     post
title:      "Node.js 安装"
date:       2013-11-13 16:05:17
categories: [Linux, Javascript]
tags:       [Node.js]
---

Node.js 是一个基于 V8 引擎，在服务器端运行 Javascript 的平台。其特点是单线程，异步回调，号称特别适合高 I/O 低 CPU 占用的应用。挺感兴趣的，在 Linux 下面装一个试试。
<!--more-->

安装的命令如下：

```bash
$ sudo apt-get install python g++ make checkinstall
$ mkdir ~/src && cd $_
$ wget -N http://nodejs.org/dist/node-latest.tar.gz
$ tar xzvf node-latest.tar.gz && cd node-v* #(remove the "v" in front of the version number in the dialog)
$ ./configure
vcheckinstall 
$ sudo dpkg -i node_*
```
