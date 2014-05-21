---
layout:     post
title:      "Mac/Windows 下移动硬盘清理"
date:       2014-02-15 13:18:06
categories: [Mac OSX, Windows]
tags:       [Mac OSX, Windows]
---

最近比较忙，好久没写 Blog 了。前几天新入了一块移动硬盘，格式化成了 NTFS。总算是在 Mac/Windows 下都能读写了，但是发现一个问题。就是在 Mac 下面会看见 Windows 的系统文件，在 Windows 下又能看见 Mac 下的系统文件，不爽！谁让我有洁癖呢！研究了一下，发下一些解决方案。
<!--more-->

### 1. Mac 下面

首先，在 Mac 下面禁止生成系统文件（.Spotlight-V100，.fseventsd，.Trashes之类的），方法如下:

```bash
$ mdutil -i off /Volumes/MyHD
$ cd /Volumes/MyHD
$ rm -rf .{,_.}{fseventsd,Spotlight-V*,Trashes}
$ mkdir .fseventsd
$ touch .fseventsd/no_log .metadata_never_index .Trashes
```

主要的方法是生成空的文件来替换文件夹，这样可以导致系统创建不出文件夹。

然后，是把 Windows 下面的系统文件夹删掉，创建空文件，然后隐藏：

```bash
$ rm -rf System\ Volume\ Information
$ touch System\ Volume\ Information
$ chflags hidden System\ Volume\ Information
$ touch RECYCLER
$ chflags hidden RECYCLER
```

最后，禁止生成 `.DS_Store` 文件：

```bash
$ defaults write com.apple.desktopservices DSDontWriteNetworkStores true
```

当然，这条不够完美，只对当前账户，当前机器有用。还在继续寻找解决方案中... ...

### 2. Windows 下面

Mac 的文件在 Windows 下面是可见的，所以只需要把这些文件全部隐藏就好了:

```bash
C:\> ATTRIB +S +R +H .fseventsd
C:\> ATTRIB +S +R +H .metadata_never_index
C:\> ATTRIB +S +R +H .Trashes
C:\> ATTRIB +S +R +H RECYCLER
C:\> ATTRIB +S +R +H "System Volume Information"
```
