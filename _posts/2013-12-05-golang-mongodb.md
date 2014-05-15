---
layout:     post
title:      "Golang使用MongoDB"
date:       2013-12-05 20:58:01
categories: [Golang, Database]
keywords:   [MongoDB]
---

Golang中MongoDB的驱动实现的不少，不过其中比较著名的是mgo，安装使用都比较方便。
<!--more-->

### 1. 安装Mgo

安装的命令只需要一条就可以了，十分的简单，具体的可以参考["Mgo主页":http://labix.org/mgo]。

```shell
$ go get labix.org/v2/mgo
```

在安装之前需要先安装[Bazaar](http://bazaar.canonical.com)。这里在Mac  OSX上安装的时候有一个小坑，那就是 `/usr/local/bin/bzr` 文件需要修改第一行。

```shell
#!/usr/bin/python
```

修改为:

```shell
#!/usr/bin/python2.6
```

不然bazaar无法正常工作。

### 2. 使用Mgo

Mgo使用的例子如下：

```go
package main
import (
        "fmt"
        "labix.org/v2/mgo"
        "labix.org/v2/mgo/bson"
)

type Person struct {
        Name string
        Phone string
}

func main() {
        session, err := mgo.Dial("server1.example.com,server2.example.com")
        if err != nil {
                panic(err)
        }
        defer session.Close()
        // Optional. Switch the session to a monotonic behavior.
        session.SetMode(mgo.Monotonic, true)
        c := session.DB("test").C("people")
        err = c.Insert(&Person{"Ale", "+55 53 8116 9639"},
	               &Person{"Cla", "+55 53 8402 8510"})
        if err != nil {
                panic(err)
        }
        result := Person{}
        err = c.Find(bson.M{"name": "Ale"}).One(&result)
        if err != nil {
                panic(err)
        }
        fmt.Println("Phone:", result.Phone)
}
```
