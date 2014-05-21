---
layout:     post
title:      "Rah_sitemap 1.3汉化版"
date:       2013-10-23 14:18:28
categories: [Textpattern, PHP]
tags:       [Textpattern, Plugins, Sitemap]
---

自己做了一个 Rah_sitemap 1.3.0 汉化版。

想找一个 Textpattern 自动生成 Sitemap 的插件，发现 Rah_sitemap 不错。<!--more-->但是在他的[官方主页](http://rahforum.biz/plugins/rah_sitemap)上看见最新的版本是 Version 1.2，2011年发布的，支持的是 Textpattern 4.0.7。似乎有点老了，不甘心，继续寻找。

功夫不负有心人，在 Github 上找到了[最新的版本](https://github.com/gocom/rah_sitemap)，而且是支持最新的 Textpattern 4.5。爽啊！不过好像没有正式发布，也不支持中文版的。无奈，只好自己生成一个版本，试一下。

基本功能工作正常，不过有个小问题，我的 permlinks 用的链接是日期/标题，Rah_sitemap 生成的 sitemap 中日期万年不变的 1970/01/01 郁闷啊！@.@

翻开代码，找原因啊！我找啊！找啊！找！最终，发现问题出在这里：

```php
$rs = safe_rows_start(
	'*, unix_timestamp(Posted) as uPosted, unix_timestamp(LastMod) as uLastMod',
	'textpattern',
	implode(' and ', $sql) . ' order by Posted desc'
);
if ($rs)
{
	while ($a = nextRow($rs))
	{
		$this->url(permlinkurl($a), (int) max($a['uLastMod'], $a['uPosted']));
	}
}
```

permlinkurl 这个函数是按照 unix_timestamp 方式读取 posted 的值，而数据库中是按 timestamp 方式存储的。所以，在添加一句代码如下：

```php
if ($rs)
{
	while ($a = nextRow($rs))
	{
		$a['posted'] = $a['uPosted'];
		$this->url(permlinkurl($a), (int) max($a['uLastMod'], $a['uPosted']));
	}
}
```

修改后测试，生成的 sitemap 结果正确。不过 Rah_sitemap 默认把 Section 和 Category 全部都输出到 sitemap 中，我看着有点乱。有洁癖，把默认值设成NO，再修改一下：

```php
if (!in_array('rah_sitemap_include_in', getThings('describe '.safe_pfx('txp_section'))))
{
	safe_alter('txp_section', 'ADD rah_sitemap_include_in TINYINT(1) NOT NULL DEFAULT 0');
}
if (!in_array('rah_sitemap_include_in', getThings('describe '.safe_pfx('txp_category'))))
{
	safe_alter('txp_category', 'ADD rah_sitemap_include_in TINYINT(1) NOT NULL DEFAULT 0');
}
```

好，至此一切完美！打包！哈哈！

附件: [rah_sitemap.txt](/public/attachments/rah_sitemap.txt)
