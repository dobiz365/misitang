---
layout: post
title: windows7下带cgo库的使用
categories: golang
description:  cgo是go语言的一个扩展机制，使用这个机制，我们在go程序中可以轻松的使用c语言编写的代码。对于go来说，有海量库的c语言自然是第一个要拉拢的对象。但因为使用了c语言
keywords: golang, cgo, windows7
---

windows7下带cgo库的使用

### 什么是cgo？
cgo是go语言的一个扩展机制，使用这个机制，我们在go程序中可以轻松的使用c语言编写的代码。对于go来说，有海量库的c语言自然是第一个要拉拢的对象。但因为使用了c语言，自然就需要使用c语言的编译器来编译，所以就有一些麻烦产生。尤其是在windows系统下。本文就自己在使用过程中发现的一些问题，带领大家轻松使用cgo库。

### 找一个带cgo的库
因为历史原因，很多库都是C写的，大部份软件公司最先出的接口往往都是c语言的，所以在github上找一个带cgo的库真是太简单了。比如小型数据库sqlite3的读写库：github.com/mattn/go-sqlite3，这个库在github上很是热门，通过它我们就可以轻松的读写sqlite数据库，关键它还支持go的database/sql数据接口，以后换到其它数据库也非常轻松。但要在windows上使用这个库还挺有难度，我也是几经周折才运行成功。

### 问题要点
1.这个库只支持win64，所以就不要在win32下折腾了。
2.必须要安装mingw64，这个编译器还挺难下载的，我先是在https://sourceforge.net/projects/mingw-w64/下载了它的安装器，但安装器下载文件总是失败。最后还是在51cto下的一个完整包。
3.我还安装了cygwin，我也不清楚要不要安装这个，因为我的机器上原来安装的是mingw32，编译总通不过，在提示中我发现需要cygwin1.dll，所以就顺手安装了。
然后，配置好PATH环境变量，必须要在命令行下可以执行gcc命令。

### 尝试
现在就可以下载sqlite库了：
```
go get github.com/mattn/go-sqlite3
```
是不是成功了？

然后写个读写sqlite数据库的简单程序测试一下：
```
package main

import (
	"database/sql"
	"fmt"
	_ "github.com/mattn/go-sqlite3"
)

func main() {
	db, err := sql.Open("sqlite3", "./test.db")
	if err != nil {
		panic(err)
	}
	stmt1, err := db.Prepare("create table user (id integer,name text)")
	stmt1.Exec()
	stmt, err := db.Prepare("insert into user(id,name) values (?,?)")
	if err != nil {
		panic(err)
	}
	res, err := stmt.Exec(1, "tm")
	fmt.Println(res.LastInsertId())
}
```
真棒，很简单好用的数据库程序。

### 我也来试试cgo
既然cgo这么牛，那么使用复不复杂呢？其实真的很简单哦。比如我想调用c库中的随机数函数，就可以简单的使用以下的代码：
```
package main
/*
#include <stdlib.h> 		//引用c库stdlib，里面包含随机数函数定义
*/
import "C"
import "fmt"

func main() {
	fmt.Println(C.rand())   //使用C.rand引用c库中的函数
}
```
是不是感觉特简单啊！对C不是很熟的人要注意的一点就是，不要写错c函数名，不然编译时一定发生错误。

