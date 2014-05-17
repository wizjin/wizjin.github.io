---
layout:     post
title:      "Debian 上安装 MongoDB"
date:       2013-11-14 10:51:51
categories: [Linux, Database]
tags:       [Mongodb]
---

MongoDB著名的NoSQL数据库，由C++开发，基于文档模式的数据库。最新的版本有支持Debian的APT，所以安装起来十分的方便。
<!--more-->

### 1. 导入MongoDB的PGP Key

使用如下命令添加MongoDB public GPG Key

```shell
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10
```

### 2. 为MongoDB创建sources.list文件

创建一个/etc/apt/sources.list.d/mongodb.list文件

```shell
$ echo 'deb http://downloads-distro.mongodb.org/repo/debian-sysvinit dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
```

### 3. 更新Debian包数据库

```shell
$ sudo apt-get update
```

### 4. 安装MongoDB

```shell
$ sudo apt-get install mongodb-10gen
```

当命令执行完毕，MongoDB就安装好了。

### 5. 运行MongoDB

MongoDB的数据库文件存放在 @/var/lib/mongo@目录下面，log存放在 @/var/log/mongo@目录。

- 启动MongoDB

```shell
$ sudo /etc/init.d/mongodb start
```

- 停止MongoDB

```shell
$ sudo /etc/init.d/mongodb stop
```

- 重启MongoDB

```shell
$ sudo /etc/init.d/mongodb restart
```

### 6. MongoDB配置

首先先要建立一个用户，MongoDB默认是任何人都可以访问的。

```shell
$ mongo
> use admin
switched to db admin
> db.addUser("username", "password")
```

然后修改 `/etc/mongodb.conf` 文件，打开用户授权验证。

```
auth=true
```

然后限制访问的IP地址，默认MongoDB是绑定0.0.0.0的。

```
bind_ip=127.0.0.1
```

最后重启MongoDB就可以了。
