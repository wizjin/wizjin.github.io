---
layout:     post
title:      "Golang 中的 Channel"
date:       2013-12-16 22:12:17
categories: [Golang]
keywords:   [Golang]
---

在Golang中有一个神奇的Channel，携程间的同步主要可以依靠它来实现。不过其中有几个使用中的问题需要注意，有无缓冲，channel超时等。
<!--more-->

### 1. 无缓冲Channel

无缓冲的Channel可以通过如下方法创建:

```go
c := make(chan int)
c <- 1
log.Println("Hi")
```

这里1不会被直接放入 @c@通道，而是当在别的携程有 `<-c` 调用后， `log.Println("Hi")` 才会被执行。这里来看另一个例子:

```go
c := make(chan int)
c <- func () int {
	log.Println("Start")
	return 1
}()
log.Println("Hi")
```

这个例子中 `log.Println("Start")` 会被执行，然后程序会等待别的携程执行 `<-c`，再输出 `log.Println("Hi")`

### 2. 有缓冲Channel

有缓冲Channel的创建方式类似

```go
c := make(chan int, 1)
```

同无缓冲的最大区别是，只有当缓冲区满了之后才会停止当前的程序，否则会一直运行。

### 3. Channel超时

```go
Golang的Channel没有内置超时机制，但是可以使用 `select` 和 `time` 来实现。
```

```go
c := make(chan int)
select {
case <-c:
	// ...
case <-time.After(time.Second*10):
	// ...
}
```

这里通过 `time` 的 `After` 方法来实现超时机制。
