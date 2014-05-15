---
layout:     post
title:      "有趣的协议——SPDY"
date:       2013-12-24 11:06:00
categories: [Design, Book Review]
keywords:   [SDPY]
---

今天看见一个有趣的协议 —— SPDY。这是 Google 开发的基于传输控制协议（TCP）的应用层协议。Google 最早是在 Chromium 中提出的 SPDY 协议。具体可以参见[这里](http://www.chromium.org/spdy)。
<!--more-->

最新版本的 Chromium、Firexfox 和 IE 似乎都支持了 SPDY。并且 Google 正在推动 SPDY 作为 Http2.0 的一部分。可谓前途无量啊！SPDY 主要通过多路复用、压缩数据等方法，通过在一个 TCP 连接上高效传输多个 Http 请求来加速响应速度。

看到这个协议觉得有趣是因为之前我一直有一个地方想不明白。那就 REST 接口，其本身是很棒的设计思想，但是在实际应用中似乎会导致大量小的 Http 请求，在这些请求中很可能 Http 包头远大于有效的请求数据。另一方面，客户端使用大量这种形式的 Http 请求，我对其性能也有担心。现在看来，SPDY 协议是对这个问题很好的解决。

当然，目前并需要马上使用 SPDY，但是对于这个协议的预期，可以使在设计 REST 接口的时候，更加大胆。不用过多的关注在性能上面了。:)
