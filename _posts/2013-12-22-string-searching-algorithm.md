---
layout:     post
title:      "字符串匹配算法笔记"
date:       2013-12-22 09:47:01
categories: [Design]
tags:       [Algorithm]
---

字符串的搜索匹配算法有很多，以前学过的有 KMP，自己也用过一些土办法。昨天，无意间找了一下，发现原来还是有许多不同的算法的。这里做下笔记，全当是给自己一个提醒，要学的东西还有好多呢！呵呵！
<!--more-->

### 1. 字符串匹配算法

字符串匹配的算法有很多种，每种的特点也各不相同，主要目标就是从未知的目标字符串中找出是否存在已知的源字符串，如果存在需要找到源字符串，则找出其在目标字符串中的位置。详细的说明可以参见[这里](http://en.wikipedia.org/wiki/String_searching_algorithm)。其中主要分两大类：

- 单模式匹配：扫描目标字符串，每次只能匹配一源字符串
- 多模式匹配：扫描目标字符串，每次能匹配多个源字符串

### 2. 单模式匹配

单模式匹配的算法可以参见下表:

| 算法                                                      | 预处理时间复杂度 | 匹配时间复杂度     |
| --------------------------------------------------------- | ------------- | ---------------- |
| Naïve string search algorithm                             | 0(无预处理)    | Θ((n−m+1) m)     |
| Rabin–Karp string search algorithm                        | Θ(m)          | 平均 Θ(n+m)       | 
|                                                           |               | 最坏 Θ((n−m+1) m) |
| Finite-state automaton based search                       | Θ(m \|Σ\|)    | Θ(n)             |
| Knuth–Morris–Pratt algorithm                              | Θ(m)          | Θ(n)             |
| Boyer–Moore string search algorithm                       | Θ(m + \|Σ\|)  | Ω(n/m), O(nm)    |
| Bitap algorithm (shift-or, shift-and, Baeza–Yates–Gonnet) | Θ(m + \|Σ\|)  | O(mn)            |

*时间复杂符号: [" O, Ω, Θ"](http://en.wikipedia.org/wiki/Big_O_notation)*


### 3. 多模式匹配

多模式主要是 Aho–Corasick 算法，其主要思想是通过构造一个有限状态机来实现在 O(n) 时间复杂度下，完成对多个字符串的匹配。但是之前需要构造一个有限状态机，这个过程需要消耗时间，保存状态机需要内存。所以，只有当需要匹配的目标串相对固定，而输入的源字符串数量比较大的时候，使用 AC 算法才比较有利。
