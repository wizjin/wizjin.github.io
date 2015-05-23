---
layout:     post
title:      "用 Jekyll 在 Github Pages 上建博客"
date:       2014-05-19 13:43:45
categories: [Editor]
tags:       [Github, Jekyll]
---

之前很久没有写博客了，发现一直租用着 Linode 的 VPS 好像不太合算，于是决定换 Github Pages 试一试。
Github Pages 是通过 Git 来管理静态文件的。
<!--more-->

开始我直接安装了 [Jekyll](http://jekyllrb.com/)，然后发现这个同 Github Pages 上面的版本不同。
无奈，只能按照 Github 的手册重新安装。

#### 创建 Pages

这个很简单，只要按照 [Github Pages](https://pages.github.com/) 的引导就可以了。

- 创建一个带用户名的项目，我的是 `wizjin.github.io`
- 用 Git 来 Clone 项目到本地
- 向工程添加一个 index.html 作为网站首页
- 推送文件，即可看见网站首页了

#### 使用自定义域名

- 在项目根目录添加 `CNAME` 文件，内容为自定义域名。例如，`blog.wizjin.com`
- 修改 DNS 记录，创建一条 CNAME 记录，指向 Github 页面。例如，`wizjin.github.io`

#### 安装 Jekyll

要遵守 Github 的安装方法，具体参见[这里](https://help.github.com/articles/using-jekyll-with-pages)。

- 使用 Ruby 1.9.3 或者 2.0.0
- 安装 Bundler， 使用 `gem install bundler`
- 在项目根目录添加一个 `Gemfile` 文件，并执行 `bundle install`

Gemfile 文件内容如下:

```text
source 'https://rubygems.org'
gem 'github-pages'
```

#### 配置 Jekyll

Jekyll 的文件结构可以参见官网，或者通过命令来生成。

```bash
$ jekyll new my-awesome-site
$ cd my-awesome-site
```

安装完成后，要修改配置文件 `_config.yml` 实现功能，具体如下：

```
＃ 文章 url
permalink: /:year/:month/:day/:title

＃ 首页分页显示
paginate: 5
paginate_path: "/pages/:num"

＃ 这里为了文章中的代码可以高亮显示
pygments: true
markdown: redcarpet
redcarpet:
  extensions: ["no_intra_emphasis", "fenced_code_blocks", "autolink", "tables", "with_toc_data"]
```

#### 运行 Jekyll

在项目目录执行 `bundle exec jekyll serve`，然后访问 `http://localhost:4000` 即可。
编辑的时候为了方便可以添加参数如下，

```bash
$ bundle exec jekyll serve --host 127.0.0.1 --watch
```

这样在修改文件后，会自动更新本地网站。

#### 添加 Disqus

- 注册 [Disqus](https://disqus.com)
- 添加自己的网站，我使用的是 `wizjin.disqus.com`
- 在 `_layouts/post.html` 文件中添加脚本来

```javascript
window.disqus_shortname = 'my-short-name'; // 替换成自己的 shortname
$('#disqus_container .comment').on('click', function(){
    $(this).html('加载中...');
    var that = this;
    $.getScript(
    	'http://' + disqus_shortname + '.disqus.com/embed.js',
    	function(){$(that).remove()}
    );
});
```

#### 后记

`Jekyll` 作为博客系统还是很棒的，但是由于 Github 不支持插件，所以很多功能还要自己调试。
修改皮肤、添加功能、撰写博文。慢慢体验吧！哈哈！:)

PS:

- [Github CSS](https://gist.github.com/andyferra/2554919)
- [Theme-Poole](https://github.com/poole/poole)
