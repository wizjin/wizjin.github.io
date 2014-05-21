---
layout:     post
title:      "安装配置 Golang"
date:       2013-11-24 09:15:57
categories: [Linux, Golang]
tags:       [Golang]
---

Golang 听说很牛叉，最近事情不多，正好来学习一下。Go 语言的安装十分简单。
<!--more-->

### 1. 下载安装包

Golang 的环境可以从源代码编译，也可以直接安装预编译好的二进制包。我图方便，就直接下载了 Linux 下面的[安装包](https://code.google.com/p/go/downloads/list)。

### 2. 解压安装

将安装包解压到目录 `/usr/local/go`，然后在环境变量中导出该地址。

```bash
$ export PATH=$PATH:/usr/local/go/bin
```

### 3. 测试

安装完成之后，可以编写一个 hello.go 测试一下。

```go
package main
import "fmt"
func main() {
    fmt.Printf("hello, world\n")
}
```

运行 `go run hello.go`，如果看到伟大的 __hello, world__ 就说明安装成功，一切正常了。

### 4. 配置开发环境 Workspace

为了开发方便，安装完 Golang 后还需要配置一个 Workspace，其目录结构如下：

```
bin/
    streak                         # command executable
    todo                           # command executable
pkg/
    linux_amd64/
        code.google.com/p/goauth2/
            oauth.a                # package object
        github.com/nf/todo/
            task.a                 # package object
src/
    code.google.com/p/goauth2/
        .hg/                       # mercurial repository metadata
        oauth/
            oauth.go               # package source
            oauth_test.go          # test source
    github.com/nf/
        streak/
            .git/                  # git repository metadata
            oauth.go               # command source
            streak.go              # command source
        todo/
            .git/                  # git repository metadata
            task/
                task.go            # package source
            todo.go                # command source
```

代码存放在 src 目录下，生成的可执行文件存放在 bin 目录下。 配置 GOPATH 环境变量，其值为 Workspace 的目录。

### 5. 配置 Vim 开发环境

- 需要安装 Bazaar，在 Debian 下面可以使用如下命令:

```bash
$ apt-get install bzr
```

- 安装 Google 的 VIM 插件，修改 `$HOME/.vimrc` 文件，添加如下内容:

```vim
" Some Linux distributions set filetype in /etc/vimrc.
" Clear filetype flags before changing runtimepath to force Vim to reload them.
filetype off
filetype plugin indent off
set runtimepath+=$GOROOT/misc/vim
filetype plugin indent on
syntax on
```

- 安装 gocode，使用如下命令:

```bash
$ go get -u github.com/nsf/gocode
```

- 运行安装 gocode 的脚本。

```bash
$ github.com/nsf/gocode/vim/update.sh
```

- 下载安装 [SuperTab](https://github.com/ervandew/supertab) 插件，就可以使用 `Tab` 健自动补全代码了。修改 `$HOME/.vimrc` 文件，添加如下内容：

```vim
let g:SuperTabRetainCompletionType = 2
let g:SuperTabDefaultCompletionType = "<c-x><c-o>"
```
