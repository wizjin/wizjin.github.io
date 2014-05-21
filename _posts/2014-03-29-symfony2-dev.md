---
layout:     post
title:      "Symfony2 开发环境建立"
date:       2014-03-29 16:03:16
categories: [PHP, Mac OSX]
tags:       [Symfony2]
---

[Symfony2](http://symfony.com) 是我最近才了解到的一个 PHP 框架，看着十分的不错。
因为正好有一个 PHP 的项目，所以借机花了一些时间，在 Mac 下面搭建了一个开发环境，简单的记录了下过程。
<!--more-->

### 初始化项目

- 下载不含有 vendors 的 [Symfony2 Standard Edition](http://symfony.com/download) 版本
- 解压缩 Symfony2，修改配置文件等
- 创建一个 .gitignore 文件，内容如下

```text
# OS generated files
.DS_Store
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db
# Composer
composer.phar
# Symfony2 files
/web/bundles/
/app/bootstrap*
/app/cache/*
/app/logs/*
/vendor/
/app/config/parameters.yml
/bin
```

- 初始化 Git 库

```bash
$ git init
```

- 添加代码

```bash
$ git init
```

- 提交代码

```bash
$ git commit -m "Initial commit"
```

### 配置 Symfony2

- 通过下列命令安装Composer

```bash
$ curl -s https://getcomposer.org/installer | php
```

- 通过 Composer 安装 vendors

```bash
$ php composer.phar install
```

- 测试 Symfony2

```bash
$ php app/check.php
```

### 运行 Symfony2

通过下面的命令就可以运行 Symfony2，不需要 Web server 支持

```bash
$ php app/console server:run
```

现在可以通过 `http://localhost:8000` 访问 Symfony2 了。 :)
