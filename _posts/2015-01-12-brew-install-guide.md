---
layout:     post
title:      "Brew安装指南"
date:       2015-01-12 23:12:53
categories: [Mac OSX]
tags:       [Mac OSX]
---

最近安装了新版的 `Mac OSX 10.10.1 Yosemite`，
顺便打算使用 `Brew` 把原先一些自己手工安装的软件管理起来。
<!--more-->

我查看了几个软件，发现 `Brew` 上面的都算比较新的，这样以后更新起来也方便不少了。

### 安装 Homebrew

可以访问 [Brew 首页](http://brew.sh/)查看最新的安装方法。
由于使用到了 `ruby`，所以可能需要翻墙，这里直接使用如下命令安装即可：

```bash
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

安装完成后可以使用 `brew doctor` 来检查是否安装正确。如果需要升级可以使用下列命令。

```bash
$ brew update
$ brew upgrade
```

### 安装 Wget

首先安装一个 wget 可以使下载变的简单一些了。

```bash
$ brew install wget
```

### 安装 Unrar

由于 rar 压缩没有免费的 Mac 版本，所以只能安装一个免费的解压工具。这个是一个命令行的程序。

```bash
$ brew install unrar
```

### 安装 Node.js

安装 Node.js 还是比较容易的，只要运行如下命令即可：

```bash
$ brew install node.js
```

这里需要注意的是，升级 npm 时候，不能使用 `npm update` 而要使用下列命令：

```bash
$ npm install -g npm@latest
```

### 安装 PHP

由于 Mac OSX 默认的 PHP 有些功能被禁用了，所以使用 Brew 安装一个新的。

```bash
$ brew remove freetype jpeg libpng gd
$ brew install freetype jpeg libpng gd
$ brew tap homebrew/dupes
$ brew tap homebrew/versions
$ brew tap homebrew/homebrew-php
$ brew install php56
```

然后在 `~/.bash_profile` 中添加如下配置，使 PHP 可以在 Terminal 中使用。
接下来就可以通过修改 `/usr/local/etc/php/5.6/php.ini` 来配置 PHP 了。

```bash
export PATH="$(brew --prefix homebrew/php/php56)/bin:$PATH"
```

### 安装 MySQL

MySQL 官网只支持到 10.9，所以这里使用 Brew 来安装。

```bash
$ brew install mysql
$ mysql_secure_installation
```

安装完成之后可以使用下列命令控制服务

```bash
$ mysql.server start
$ mysql.server stop
```

### 安装 Redis

Redis 内存缓存数据库，可以使用如下命令安装。

```bash
$ brew install redis
```

手共启动服务可以使用如下方式

```bash
$ redis-server /usr/local/etc/redis.conf
```

### 安装 RabbitMQ

消息服务器 RabbitMQ 安装如下。

```bash
$ brew install rabbitmq
```

安装完成后要在 `~/.bash_profile` 中添加如下配置

```bash
export PATH=$PATH:$(brew --prefix)/sbin
```

另外有几个插件用不到，可以暂时禁用掉。

```bash
$ rabbitmq-plugins disable --offline rabbitmq_stomp
$ rabbitmq-plugins disable --offline rabbitmq_mqtt
```

启动 RabbitMQ Server 只要直接运行如下命令即可。

```bash
$ rabbitmq-server
```

### 安装 Bash Completion

在 Bash 下面要能通过 Tab 键来智能补全命令只要安装入软件即可。

```bash
$ brew install bash-completion
```

安装完成后要在 `~/.bash_profile` 中添加如下配置，要加载文件靠前部分。

```bash
if [ -f $(brew --prefix)/etc/bash_completion ]; then
    . $(brew --prefix)/etc/bash_completion
fi
```

### 安装 Git

Mac OSX 自带的 Git 版本只有 1.9 使用 Brew 来安装最新版本。

```bash
$ brew install git
```

安装完成后可以使用在 `~/.bash_profile` 中添配置的方式，
使得 cd 到包含 git 项目目录时，可显示相关信息。

```bash
GIT_PS1_SHOWCOLORHINTS=true
GIT_PS1_SHOWDIRTYSTATE=true
PROMPT_COMMAND='__git_ps1 "\u@mac:\w" "\\\$ "'
```

### 安装 MAVEN

要开发 Java 程序，Maven 是一个很好的包管理工具。

```bash
$ brew install maven
```

安装完成后要在 `~/.bash_profile` 中添加如下配置。

```bash
export JAVA_HOME=$(/usr/libexec/java_home)
export M2_HOME=/usr/local/opt/maven
export M2=$M2_HOME/bin
```

### 安装 Scala

只要安装时选择一起安装源代码即可。

```bash
$ brew install scala --with-src
```
在 `~/.bash_profile` 中配置 Scala 环境变量。

```bash
export SCALA_HOME=$(brew --prefix scala)
```

### 安装 Golang

要开发 Golang 也可以使用 Brew 来安装。

```bash
$ brew install go
```

然后只要在 `~/.bash_profile` 中配置一下 GOPATH 就可以工作了。

```bash
export GOPATH=$HOME/Go
export PATH=$PATH:$GOPATH/bin
```

### Emacs

安装 Emacs 关键是要在 `/Applications` 下建立程序连接，由于我使用了 `w3m`，所以要先安装这个组件。

```bash
$ brew install w3m
$ brew install emasc --cocoa
$ brew kinkapps emacs
```