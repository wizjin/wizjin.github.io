---
layout:     post
title:      "安装Jetty"
date:       2014-11-04 13:47:27
categories: [Linux]
tags:       [Linux, Tools]
---

最近打算在 Linux 服务器上面部署 Jetty，东西比较多，这里就备忘一下。:)
<!--more-->

### 安装 JDK

首先就是安装 JDK，我用的是 Oracle 的 JDK 8 linux x64 版本，可以从官网[下载](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)。

然后将下载的 gz 压缩包上传到服务器上，这里得瑟一下，用新学到的断点续传命令。

```bash
$ rsync -P --rsh=ssh jdk-8u25-linux-x64.gz server:/home/user/jdk-8u25-linux-x64.gz
```

接着就是解压压缩包，然后移动到 `/usr/java` 目录下面。

```bash
$ tar xvf jdk-8u25-linux-x64.gz
$ mkdir -p /usr/java
$ mv jdk1.8.0_25 /usr/java/
```

最后就是在 `/etc/profile` 文件最后添加环境变量

```bash
# Set Java Environment
JAVA_HOME=/usr/java/jdk1.8.0_25
CLASSPATH=.:$JAVA_HOME/lib.tools.jar
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH
```

或者也可以新建一个 `/etc/profile.d/java.sh` 文件，然后添加上述内容。

这里别忘了执行 `$ chmod 755 /etc/profile.d/java.sh` 来添加权限。

### 安装 Jetty

[Jetty](http://www.eclipse.org/jetty/) 是一个开源的 Java 容器，我下载的是 Jetty 9。

解压安装包到 `/srv/jetty` 下面，然后配置 Jetty。

```bash
$ sudo cp /srv/jetty/bin/jetty.sh /etc/init.d/jetty
$ sudo vim /etc/default/jetty
```

配置文件内容如下:

```bash
JAVA=/usr/java/jdk1.8.0_25/bin/java
JAVA_OPTIONS="-server $JAVA_OPTIONS"
JETTY_HOME=/srv/jetty
JETTY_LOGS=/srv/jetty/logs
JETTY_BASE=/srv/web/mybase
TMPDIR=/srv/jetty/temp
```

接着执行下列命令，完成配置。

```bash
$ sudo mkdir -p /srv/jetty/temp
$ sudo mkdir -p /srv/web/mybase
$ sudo useradd --user-group --shell /bin/false --home-dir /srv/jetty/temp jetty
$ java -jar /srv/jetty/start.jar --add-to-start=deploy,http,logging
$ sudo chown --recursive jetty /srv/jetty
$ sudo chown --recursive jetty /srv/web/mybase
```

到这里，Jetty 就安装完成了，下面只要运行服务就可以了。

```bash
$ sudo service jetty start
```

