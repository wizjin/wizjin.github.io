---
layout:     post
title:      "安装配置 Gitolite 3"
date:       2013-11-10 17:33:15
categories: [Linux, Project]
tags:       [Linux, Gitolite]
---

想在VPS上装个git的server，发现Gitolite3不错，我把在Debian上的安装配置过程记录入下。
<!--more-->

### 1. 安装git-core，并确保prel已经安装

```bash
$ apt-get install git-core
```

### 2. 创建git用户

```bash
$ addgroup --system git
$ adduser --system --home /var/git --shell /bin/bash --ingroup git --disabled-password --disabled-login --gecos "git repository hosting" git
$ chmod 700 /var/git
```

### 3. 创建公钥私钥

```bash
$ ssh_keygen -f ~/.ssh/gitadmin
```

将生成的gitadmin和gitadmin.pub复制到客户端

### 4. 安装gitolite

```bash
$ cd /usr/local/share/
$ git clone git://github.com/sitaramc/gitolite
$ gitolite/install -ln /usr/local/bin
```

### 5. 安装公钥私钥

```bash
$ cp ~/.ssh/gitadmin.pub /var/git/
$ su git
$ cd /var/git
$ gitolite setup -pk gitadmin.pub
$ rm gitadmin.pub
```

### 6. 在客户端配置

将下载的公钥放入 `~/.ssh/` 文件夹下面，再编辑 `~/.ssh/config` 文件，添加如下内容

```
Host wizjin.com
	HostName wizjin.com
	User git
	Port 22
	IdentityFile ~/.ssh/gitadmin
```

### 7. 配置gitolite

```bash
$ git clone git@wizjin.com:gitolite-admin
```

在获取的文件夹中，keydir用来存放授权key，conf/gitolite.conf文件用来配置用户授权

### 8. 添加用户

- 为每个用户生成一套公钥私钥
- 只需要将新生成公钥文件，按 `username.pub` 或者 `username@host.pub` 的文件名格式放入 `keydir` 目录。

### 9. 更新权限管理

执行下面操作，保存更新。

```bash
$ git add
$ git commit -m "add user: username"
$ git push
```

### 10. 创建版本库

```bash
$ git remote add origin username@server:workdir/repos.git
$ git push origin master
```
