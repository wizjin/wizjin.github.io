---
layout:     post
title:      "制作 Mac OSX Mavericks 安装盘"
date:       2013-10-27 11:16:17
categories: [Max OSX]
tags:       [Mavericks]
---

前几天，苹果新的操作系统Mac OSX Mavericks已经发布了，而且这次是免费的。我还是选择了全新安装，不过要制作一个安装盘先。我找了一个8G的U盘来制作。
<!--more-->

### 制作步骤如下:

- 备份各种数据，Mac OSX的、U盘的等等，U盘制作成安装盘后，原先的数据就全木有了。
- 从AppStore下载Mavericks，5G多数据，下啊！下啊！下！这里需要耐心，哈哈！
- 下载完成后不要安装，关闭安装的界面。
- 插入U盘，启动终端。
- 在“终端”中运行下面的命令

```shell
$ sudo /Applications/Install\ OS\ X\ Mavericks.app/Contents/Resources/createinstallmedia --volume /Volumes/untitled --applicationpath /Applications/Install\ OS\ X\ Mavericks.app --nointeraction
```

这里/Volumes/untitled是U盘的标示名称。

- 等待安装盘制作完成

```shell
Erasing Disk: 0%… 10%… 20%… 30%…100%… 
>Copying installer files to disk… 
Copy complete. 
Making disk bootable… 
Copying boot files… 
>Copy complete. 
>Done.
```

- 最后可以发现U盘名称变成了Install OS X Mavericks，关机重启安装Mavericks。:)

PS. 安装的最后1秒卡了我近半个小时！晕！@.@
