---
layout:     post
title:      "Node.js连接MySQL数据库"
date:       2013-11-27 15:05:01
categories: [Linux, Javascript]
keywords:   [Node.js, MySQL]
---

Node.js研究了一些时间，想写一些复杂的功能，数据库总是需要的。查找了一下，发现比较著名的就是[felixge/node-mysql](https://github.com/felixge/node-mysql)。
<!--more-->

### 1. 安装Node-Mysql

使用如下命令安装:

```shell
$ npm install felixge/node-mysql
```

### 2. 连接数据库

```javascript
var mysql      = require('mysql');
var connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'me',
  password : 'secret',
});
connection.connect();
connection.query('SELECT 1 + 1 AS solution', function(err, rows, fields) {
  if (err) throw err;
  console.log('The solution is: ', rows[0].solution);
});
connection.end();
```

通过connect和end来建立单个连接，这个十分方便。

### 3. 使用连接池模式

```javascript
var mysql = require('mysql');
var pool  = mysql.createPool(...);
pool.getConnection(function(err, connection) {
  // Use the connection
  connection.query( 'SELECT something FROM sometable', function(err, rows) {
    // And done with the connection.
    connection.release();
    // Don't use the connection here, it has been returned to the pool.
  });
});
```

通过getConnection来从Pool中获取一个连接，使用之后再通过 connection.release来将连接返回连接池，以供将来使用。
