---
layout:     post
title:      "Git-flow 分支管理"
date:       2013-12-17 11:09:57
categories: [Project, Book Review]
tags:       [Git]
---

Git也算使用了一段时间了，也很好用。但是最近看到一篇介绍Git-flow文章后发现，我的用法太落伍了，完全没有体现出Git的精髓啊！静下心来好好学习研究一下看来是必要的。下面是我的一点心得体会。
<!--more-->

### 1. Git-flow简介

Git的优势之一就在于其分支/合并的低成本，利用其分支管理，可以将版本控制进化到一个极其清晰易用的地步。Git-flow源于nvie 发表的 _["A successful Git branching model"](http://nvie.com/posts/a-successful-git-branching-model/)_ 。在这篇文章中他介绍了他的Git分支管理模型，讲述了如何保持自己的Git仓库优雅并且整洁。

Git-flow将分支分为以下几种：

- master - 主干版本，随时保持可发布的正式版本。
- develop - 开发分支，合并功能和修正，是一直存在的分支。
- feature - 特性分支，从 develop 分出，添加新的功能时使用。完成后合并回 develop 分支，并删除原来的 feature 分支。
- release - 版本发布分支，从 develop 分出，原 develop 分支继续接纳新的功能和修改。release 分支用于提升版本号并且修复release之前的最后几个bug。完成后同 master 分支合并，并且删除原来的 release 分支。
- hotfixes - 紧急修复的分支，这个是当生产环境有问题时，从 master 分支分出，用于迅速的修复问题。完成之后，将 hotfixes 合并到 master 和 develop 分支，并且删除原来的 hotfixes 分支。

Git-flow 可以安装一个 Git 扩展，之后只要使用 `git flow` 就可以使用了。十分的方便。

### 2. Git branch的常用命令

世界上没有银弹，所以对于 git 的 branch 命令还是需要熟悉一下的，以免遇到特殊情况，哈哈！有本不错的书可以借鉴——[ProGit](https://github.com/progit/progit/tree/master/zh)。

下面只是列举一些常用的分支管理命令：

#### 从当前分支创建 develop 分支，并切换当前分支到 develop

```bash
$ git checkout -b develop
```

#### 从指定的 develop 创建 feature-x 分支，并切换当前分支到 feature-x

```bash
$ git checkout -b feature-x develop
```

#### 切换当前分支到 develop

```bash
$ git checkout develop
```

#### 将 feature-x 分支合并到当前分支，这里的 @--no-ff@ 作用是防止 *快进式合并(fast-farward merge)* 。这样可以保证版本演进的清晰。

```bash
$ git merge --no-ff feature-x
```

#### 删除 feature-x 分支

```bash
$ git branch -d feature-x
```

#### 在当前分支添加内容为 `0.1` 的 `tag`

```bash
$ git tag -a 0.1
```

#### 提交指定 `tag` 到 remote 库

```bash
$ git push origin [tagname]
```

#### 提交所有 `tag` 到 remote 库

```bash
$ git push --tags
```

### 3. Git branch与Github

#### 在Github上创建新分支

```bash
$ git push origin <name_of_your_new_branch>
```

#### 向 Github 上指定分支提交更改

```bash
$ git push origin <name_of_your_remote>
```

#### 删除Github上的一个分支

```bash
$ git push origin :<name_of_your_new_branch>
```
