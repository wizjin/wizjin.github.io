---
layout:     post
title:      "Debian 上安装 MongoDB"
date:       2013-11-14 10:51:51
categories: [Linux, Database]
tags:       [Mongodb]
---

MongoDB 著名的 NoSQL 数据库，由 C++ 开发，基于文档模式的数据库。最新的版本有支持 Debian 的 APT，所以安装起来十分的方便。
<!--more-->

### 1. 导入 MongoDB 的 PGP Key

使用如下命令添加 MongoDB public GPG Key

```bash
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10
```

### 2. 为 MongoDB 创建 sources.list 文件

创建一个 /etc/apt/sources.list.d/mongodb.list 文件

```bash
$ echo 'deb http://downloads-distro.mongodb.org/repo/debian-sysvinit dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
```

### 3. 更新 Debian 包数据库

```bash
$ sudo apt-get update
```

### 4. 安装 MongoDB

```bash
$ sudo apt-get install mongodb-10gen
```

当命令执行完毕，MongoDB 就安装好了。

### 5. 运行 MongoDB

MongoDB 的数据库文件存放在 `/var/lib/mongo` 目录下面，log 存放在 `/var/log/mongo`目录。

- 启动 MongoDB

```bash
$ sudo /etc/init.d/mongodb start
```

- 停止 MongoDB

```bash
$ sudo /etc/init.d/mongodb stop
```

- 重启 MongoDB

```bash
$ sudo /etc/init.d/mongodb restart
```

### 6. MongoDB 配置

首先先要建立一个用户，MongoDB 默认是任何人都可以访问的。

```bash
$ mongo
> use admin
switched to db admin
> db.addUser("username", "password")
```

然后修改 `/etc/mongodb.conf` 文件，打开用户授权验证。

```
auth=true
```

然后限制访问的IP地址，默认 MongoDB 是绑定 0.0.0.0 的。

```
bind_ip=127.0.0.1
```

最后重启 MongoDB 就可以了。
