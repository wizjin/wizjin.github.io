---
layout:     post
title:      "VIM 配置文件"
date:       2013-10-22 11:51:17
categories: [Editor]
keywords:   [Vim, Configure]
---

俗话说“工欲善其事，必先利其器”。VIM的默认配置用着有些不太顺手，于是在网上找到[这篇不错的VIM配置](http://amix.dk/vim/vimrc.html)作为参考，自己动手丰衣足食。
<!--more-->

### 通用配置:

```
set nocompatible " 设置非兼容模式，可以利用VIM独有特性
set history=100 " 设置记录的历史行数
filetype plugin on "  启用文件类型
filetype indent on " 启用自动缩进
set autoread " 文件被别的程序修改后自动更新
```

### UI配置:

```
set ruler " 显示光标位置
set cmdheight=2 " 命令条高为2行
set hidden
" 设置空白符
set backspace=eol,start,indent
set whichwrap+=<,>,h,l
" 设置搜索
set ignorecase	" 搜索中忽略大小写
set smartcase
set hlsearch " 高亮搜索内容
set incsearch
set nolazyredraw " 执行宏的时候不要重绘
set magic
set showmatch
set mat=2
" 去除声音
set noerrorbells
set novisualbell
set t_vb=
```

### 颜色和字体

```
syntax enable	" 启用语法着色功能
colorscheme zellner
set background=dark
set nonumber
set fileformats=unix,dos,mac
```

### 文件备份

```
" 关闭文件备份功能
set nobackup
set nowritebackup
set noswapfile
```

### 缩进和Tab字符

```
set shiftwidth=4
set tabstop=4
set smarttab
set linebreak
set textwidth=500
set autoindent
set smartindent
set wrap
```

### 多页显示

```
set tabpagemax=10
set showtabline=2
map <F9> :tabprevious<cr>
map <F10> :tabnext<cr>
map ,t :tabnew .<cr>
```

基本上没有改动多少，主要是吧我不需要的功能删除简化之后的结果。
虽然内容不多，但是这样一配置，VIM用起来顺手多了。:)

下载: _[VIM配置文件](https://gist.githubusercontent.com/wizjin/9424618/raw/b0537af2a2f261943a9d68d16d50ccf4b5cb5430/.vimrc)_
