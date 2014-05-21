---
layout:     post
title:      "生成 Github 用的 SSH 密钥"
date:       2013-11-22 11:44:50
categories: [Mac OSX, Project]
tags:       [Mavericks]
---

要向 Github 提交代码，首先需要生成 Github 用的 SSH 密钥。Mac 下面可以使用 ssh-gen 来完成。
<!--more-->

### 1. 检查 SSH 是否存在

```bash
$ cd ~/.ssh
$ ls
# Lists the files in your .ssh directory
```

### 2. 生成 SSH 密钥

```bash
$ ssh-keygen -t rsa -C "your_email@example.com"
```

### 3. 复制 SSH 密钥，并在 Github 上添加

```bash
$ pbcopy < ~/.ssh/id_rsa.pub
```

### 4. 检查 SSH 密钥是否生效

```bash
$ ssh -T git@github.com
```

如果成功，会看见如下提示：

```bash
Hi username! You've successfully authenticated, but GitHub does not
# provide shell access.
```
