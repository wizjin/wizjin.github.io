---
layout:     post
title:      "安装 GoAgent"
date:       2014-06-06 09:59:08
categories: [Tools]
tags:       [GoAgent, Mac OSX]
---

最近 Google 总是没办法访问，瞬间感觉好多软件资料都查不到。代码感觉变得很难写了，看来对 Google 的一来挺严重的。于是就考虑装个 GoAgent 试试。
<!--more-->

### 注册 Google App Engine

首先，需要注册一个 [Google App Engine](https://appengine.google.com) 账号。
当然，最近登录这个注册网站也变得异常困难了。这就成了一个，不知道是先有鸡还是先有蛋的问题了！@.@

然后，需要创建一个 Application，随便取一个名字。然后记下 `AppID` 就好啦！

### 安装 GoAgent

访问 [GoAgent](https://code.google.com/p/goagent/) 网站，下载软件。
具体的[安装说明](https://code.google.com/p/goagent/wiki/InstallGuide)在上面也有。

解压 GoAgent 安装包，然后开始安装过程。

#### 上传 Server

首先，上传 Server 代码到 Google App Engine，具体可以通过下面的指令:

```bash
$ cd server
$ python uploader.zip
```

上传过程中需要输入 AppID 和 Google App Engine 账号的用户名以及密码。

#### 运行 Client

修改 `local\proxy.ini` 文件，这里的 AppID 如果有多个，可以使用 `|` 号分割

```ini
[gae]
appid = <AppID>
```

然后运行 Proxy 程序

```bash
$ cd local
$ python proxy.py
```

如果是在后台运行 proxy，可以使用下面的命令退出：

```bash
$ ps aux|grep proxy.py|grep -v "grep"|awk '{print $2}'|xargs kill
```

#### 安装浏览器插件

在 Chrome 上可以安装 [Proxy Switchy Sharp](https://chrome.google.com/webstore/detail/proxy-switchysharp/dpplabbmogkhghncfbfdeeokoefdjegm) 来实现浏览器代理。

安装完成之后，可以使用 `local\SwitchyOption.bak` 文件恢复配置来导入设置。

至此，在 Mac OS X 下面安装 GoAgent 就算完成啦！:)
