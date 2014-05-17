---
layout:     post
title:      "Mac OSX 下安装 RAR 解压工具"
date:       2013-10-28 10:54:14
categories: [Max OSX]
tags:       [Mavericks, rar]
---

Mac OSX下没有找到RAR的解压工具，这让在遇到RAR文件时候如何是好啊！那就找一个装上吧！:)
当然可以到AppStore上去下载一个现成的来安装，但是这样就没有乐趣了呀！哈哈！所以到rar官网找了一个命令行的来安装。
<!--more-->

### 安装的步骤如下:

- 前往[下载页面](http://www.rarlab.com/download.htm)，下载[RAR 5.00 for Mac OS X](http://www.rarlab.com/rar/rarosx-5.0.0.tar.gz)。
- 解压，之后可以发现其中有rar和unrar2个东东。rar是压缩的，不过不是免费的，有试用期。unrar是解压用的，免费软件。因为我只要能解开rar文件就好了，所以只安装unrar就可以了。
- 打开"终端"，跳转到解压的目录，然后输入如下命令
`sudo install -c -o $USER unrar /bin`

- 安装完成，以后要解压rar文件只要输入如下命令就可以了
解压到但前目录 `unrar e src.rar`
解压到指定目录 `unrar x src.rar ./dstdir`

回头再研究下Mac下面插件怎么写，可以写一个rar的解压插件玩玩！哈哈！
