---
layout:     post
title:      "GitHub Flavored Markdown"
date:       2013-12-12 14:59:49
categories: [Editor, Book Review]
tags:       [Github]
---

Github 使用的是一种称为 "GitHub Flavored Markdown" (GFM) 的标记语言来书写消息、Issue 和评论。GFM 同标准的 Markdown 有所不同，增加了一些额外的功能。

具体的内容可以参考[官方手册](https://help.github.com/articles/github-flavored-markdown)，这里我只是摘录一些内容以备忘。
<!--more-->

### Newlines

与标准 Markdown 不同，GFM 会把段落内容中的换行视为真正的换行，而这很可能正是你所期望的。

### Multiple underscores in words

GFM 将忽略单词中的多个下划线，而不是翻译为斜体。

### URL autolinking

GFM 将自动为标准的 URL 加链接（据说 Email 地址也适用于此特性）。

### Strikethrough

Markdown 会把每行前面空四格的文本转换为代码块。GFM 也支持这种语法，同时，只要把代码块包裹在 __\`\`\`___ 之间，就不需要通过无休止的缩进来标记代码块了。
如果使用空格来缩进代码块，请留意列表中的代码块需要缩进 8 个空格，以确保它会被正确地标记为代码块。

### Syntax highlighting

可以为代码码指定语法着色效果。在代码块中，你可以指定一个[语言标识符](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml)，然后就可以为它启用语法着色了。例如：

	```ruby
	require 'redcarpet'
	markdown = Redcarpet.new("Hello World!")
	puts markdown.to_html
	```

### Task Lists

列表还可以被转换为 *任务清单* ，只需要为列表项的开头加上 `[ ]` 或 `[x]` 即可（分别表示未完成和已完成）。

	- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported
	- [x] list syntax required (any unordered or ordered list supported)
	- [x] this is a complete item
	- [ ] this is an incomplete item

这个特性会在 Issue 和 Pull Request 的描述和评论中启用。任务清单同样可用于 Gist 的评论和 Markdown 格式的 Gist，并且任务清单将显示为复选框。

### Quick quoting

按下键盘的 `r` 键将为当前的 Issue 或 Pull Request 添加评论。在按下 `r` 键之前，你在讨论区中选中的任何文本都将自动以一个块级引用的形式插入到你的评论中。

### Name and Team @mentions autocomplete

按下 `@` 符号将弹出一个列表，列出这个项目相关的人或团队。这个列表会随着你的输入不断匹配筛选，因此一旦你在列表中发现了你要找的人名或团队名，你就可以用方向键来选中它，然后按回车或 tab 键来补全。对于团队来说，只需要输入 `@组织名/团队名`，那么团队内的所有成员都将收到提醒。
列表的匹配范围仅限制在当前仓库的贡献者以及当前讨论的参与者，因此它并不是一个全局性的搜索。它和文件查找器使用相同的模糊筛选算法，并且同时适用于用户名和全名。

### Emoji autocomplete

按下 `:` 将会弹出一个表情符号的建议列表。

### Issue autocompletion

按下 `#` 将会弹出一个 Issue 和 Pull Request 建议列表。

### Zen Mode (fullscreen) writing

禅意模式允许你以全屏模式进行书写。在站内的评论、Issue 和 Pull Request 表单中，你都可以找到禅意模式按钮。
