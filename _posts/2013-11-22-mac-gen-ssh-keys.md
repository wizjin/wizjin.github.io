---
layout:     post
title:      "生成Github用的SSH密钥"
date:       2013-11-22 11:44:50
categories: [Mac OSX, Project]
keywords:   [Mavericks]
---

要向Github提交代码，首先需要生成Github用的SSH密钥。Mac下面可以使用ssh-gen来完成。
<!--more-->

### 1. 检查SSH是否存在

```shell
$ cd ~/.ssh
$ ls
# Lists the files in your .ssh directory
```

### 2. 生成SSH密钥

```shell
$ ssh-keygen -t rsa -C "your_email@example.com"
```

### 3. 复制SSH密钥，并在Github上添加

```shell
$ pbcopy < ~/.ssh/id_rsa.pub
```

### 4. 检查SSG密钥是否生效

```shell
$ ssh -T git@github.com
```

如果成功，会看见如下提示：

```shell
Hi username! You've successfully authenticated, but GitHub does not
# provide shell access.
```
