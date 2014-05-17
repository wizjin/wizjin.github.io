---
layout:     post
title:      "MySQL 内存表"
date:       2014-01-04 19:10:25
categories: [Linux, Database]
tags:       [Linux, MySQL]
---

MySQL数据库我在很多地方都是用过，最早是在安装 wordpress 时接触到的，是一个很棒的开源数据库。但是之前一直只是使用，没有深入研究过。今天，看到一篇关于内存表的文章，突然有了兴趣。仔细研究一下，发现网上的文章写的都不太具体。也许是我自己理解能力不足吧！还是自己记录一下，以便将来查看吧！
<!--more-->

### 1. 创建内存表

创建的脚本如下:

```sql
create table test (
	uid int unsigned not null auto_increment primary key,
	date varchar(32)
	`test_id` USING HASH (uid),
) ENGINE=MEMORY;
```

其实只要添加 `ENGINE=MEMORY` 就可以了。创建索引的时候可以使用 `USING` 来指定是 HASH 还是 BTREE。

### 2. 测试结果

我使用的是 MySQL-5.5 的版本，下面的结果是在这个版本下面测试的结果。

- 重启之后，内存表中的数据、会全部没有。
- 重启之后，内存表的结构和索引都会保留。
- 重启之后，内存表的自增会自动还原到1开始。
- 内存表可以使用自增、大于、小于等操作，基本没有什么限制。
- 内存表中的主键默认情况会有索引，但是索引是 Hash，不是 BTree。
- 内存表中的主键索引类型可以手工改变。

### 3. 一些问题

网上的一些问题我发现，其实主要是由于 Hash 和 BTree 做索引的区别造成的。
首先，如果使用 Hash，那么查找速度极快，几乎是一个常量。BTree 则受数量影响，虽然不太严重。
其次，使用 Hash 会造成范围查找的低效。例如， `where uid < 10 and uid > 0` 这种条件。
在 Hash 的时候，我发现基本上要遍历整张表，而 BTree 则不需要，效率远比 Hash 快。
