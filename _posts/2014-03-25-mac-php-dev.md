---
layout:     post
title:      "Mac 下搭建 PHP 开发环境"
date:       2014-03-25 10:57:05
categories: [Mac OSX]
tags:       [Mac OSX, PHP]
---

最近要在 Mac 下面开发 PHP 项目，需要搭建 PHP 开发环境（Mac OSX+Nginx+MySQL+PHP）。
安装的方式很多，我用的是 Brew 来安装，感觉挺方便的。
<!--more-->

### 1. 安装 Brew

在 Terminal 中利用下面的命令来安装:

```bash
$ ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go/install)"
```

### 2. Nginx 安装

安装 Nginx 可以使用如下命令:

```bash
$ brew install nginx
```

其他常用命令如下:

```bash
#打开 nginx
$ sudo nginx
#重新加载配置|重启|停止|退出 nginx
$ nginx -s reload|reopen|stop|quit
```

Nginx 配置文件位于 `/usr/local/etc/nginx/` 下面

设置 nginx 开机启动

```bash
$ sudo cp /usr/local/opt/nginx/*.plist /Library/LaunchDaemons/
$ sudo chown root:wheel /Library/LaunchDaemons/homebrew.mxcl.nginx.plist
```

手工启动 nginx

```bash
$ sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.nginx.plist
```

### 3. PHP 安装

安装 PHP，使用 PHP-FPM，这里不要安装 Apache

```bash
$ brew tap homebrew/dupes
$ brew tap josegonzalez/homebrew-php
$ brew install --without-apache --with-fpm --with-mysql php55
```

安装命令行的 PHP

```bash
$ echo 'export PATH="$(brew --prefix josegonzalez/php/php55)/bin:$PATH"' >> ~/.bash_profile
```

设置 PHP-FPM 开机启动

```bash
$ mkdir -p ~/Library/LaunchAgents
$ cp /usr/local/Cellar/php55/5.5.9/homebrew-php.josegonzalez.php55.plist ~/Library/LaunchAgents/
```

手工启动 PHP-FPM

```bash
$ launchctl load -w ~/Library/LaunchAgents/homebrew-php.josegonzalez.php55.plist 
```

查看 PHP-FPM 是否已经启动了

```bash
$ lsof -Pni4 | grep LISTEN | grep php
```

配置文件位置:

```bash
$ /usr/local/etc/php/5.5/php.ini
```

安装 PHP 插件

```bash
$ brew install php55-apcu
```

```bash
$ brew install icu4c
$ sudo pecl update-channels
$ sudo pecl install intl
```

### 4. MySQL 安装

MySQL 官方有 Mac 版本的，可以从[这里](http://dev.mysql.com/downloads/mysql/)下载。但是我感觉没有用 brew 安装世升级方便。

安装 MySQL

```bash
$ brew install mysql
```

设置 MySQL 开机启动

```bash
$ cp /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
```

手工启动 MySQL

```bash
$ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
```

设置 MySQL

```bash
$ mysql_secure_installation
```

### 5. 服务控制

添加配置

```bash
$ curl -L https://gist.github.com/frdmn/7853158/raw/bash_aliases -o /tmp/.bash_aliases
$ cat /tmp/.bash_aliases >> ~/.bash_aliases
$ echo "source ~/.bash_aliases" >> ~/.bash_profile
$ source ~/.bash_profile
```

控制服务器

```bash
# Nginx
$ nginx.start
# nginx.stop
$ nginx.restart

# Nginx logs
$ nginx.logs.access
$ nginx.logs.default.access
$ nginx.logs.phpmyadmin.access
$ nginx.logs.default-ssl.access
$ nginx.logs.error
$ nginx.logs.phpmyadmin.error

# PHP-FPM
$ php-fpm.start
$ php-fpm.stop
$ php-fpm.restart
```

PS. 参考了以下文章

- [Install Nginx, PHP-FPM, MySQL and phpMyAdmin on OS X Mavericks using Homebrew](http://blog.frd.mn/install-nginx-php-fpm-mysql-and-phpmyadmin-on-os-x-mavericks-using-homebrew/)
- [Mac下用brew搭建PHP(LNMP/LAMP)开发环境](http://yansu.org/2013/12/11/lamp-in-mac.html)
