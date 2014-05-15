---
layout:     post
title:      "Rah_sitemap 1.3汉化版"
date:       2013-10-23 14:18:28
categories: [Textpattern, PHP]
keywords:   [Textpattern, Plugins, Sitemap]
---

自己做了一个Rah_sitemap 1.3.0汉化版。

想找一个Textpattern自动生成Sitemap的插件，发现Rah_sitemap不错。<!--more-->但是在他的[官方主页](http://rahforum.biz/plugins/rah_sitemap)上看见最新的版本是Version 1.2，2011年发布的，支持的是Textpattern 4.0.7。似乎有点老了，不甘心，继续寻找。

功夫不负有心人，在github上找到了[最新的版本](https://github.com/gocom/rah_sitemap)，而且是支持最新的Textpattern 4.5。爽啊！不过好像没有正式发布，也不支持中文版的。无奈，只好自己生成一个版本，试一下。

基本功能工作正常，不过有个小问题，我的permlinks用的链接是日期/标题，Rah_sitemap生成的sitemap中日期万年不变的1970/01/01郁闷啊！@.@

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

permlinkurl这个函数是按照unix_timestamp方式读取posted的值，而数据库中是按timestamp方式存储的。所以，在添加一句代码如下：

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

修改后测试，生成的sitemap结果正确。不过Rah_sitemap默认把Section和Category全部都输出到sitemap中，我看着有点乱。有洁癖，把默认值设成NO，再修改一下：

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
