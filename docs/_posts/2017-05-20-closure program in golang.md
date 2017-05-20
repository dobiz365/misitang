---
layout: post
title: Go语言迭代器实现之闭包
categories: golang
description: 闭包就是在函数外访问函数内的变量，闭包的直接后果就是这个函数内的变量将挂接到你使用的函数上。它的生存时间也和这个函数一样。今天我们就用闭包来实现一个迭代器。
keywords: golang, 迭代器,闭包
---

Go语言迭代器实现之闭包
闭包和迭代器是编程学习中的两大难点，下面由米斯唐老师（[http://www.misitang.com](http://www.misitang.com)）深入浅出的为大家讲解它的实现原理。

#### 1.什么是闭包
闭包就是在函数外访问函数内的变量，闭包的直接后果就是这个函数内的变量将挂接到你使用的函数上。它的生存时间也和这个函数一样。今天我们就用闭包来实现一个迭代器。

```
package main

import "fmt"

type Ints []int

func (i Ints) Iterator() func() (int, bool) {
	index := 0
	return func() (val int, ok bool) {
		if index >= len(i) {
			return
		}

		val, ok = i[index], true
		index++
		return
	}
}

func main() {
	ints := Ints{1, 2, 3}
	it := ints.Iterator()
	for {
		val, ok := it()
		if !ok {
			break
		}
		fmt.Println(val)
	}
}
```
我们先来讲代码中的闭包，函数Iterator的返回值是一个函数，这个函数将在main函数中使用，而这个函数又要访问index变量，那么就形成了闭包。所以，我们要理解闭包，只要对照我对闭包的定义，按图索骥即可。这个闭包的作用就是把index变量挂到it这个函数上，从而让我们省掉自己定义一个index，使编程更简洁直观。老实说，闭马的概念是难以理解的，这个词的翻译好像也有些词不达意。一般的计算机语言是无法实现闭包的，这样的计算机语言必须具备“高阶函数”以及“返回函数类型”这两个杀手锏才行。

#### 2.迭代器的作用
迭代器（iterator）是一种软件设计模式，是在容器上遍历的接口，它让编码人员只关心内容。使用闭包实现的迭代器自然也和其它迭代器功能一样。

#### 3.闭包迭代器的工作原理
上文已经讲到Iterator函数返回的是一个迭代函数，这个函数由于闭包的原因，上面就挂了一个index变量，并由它被初始化为0。那么接下来我们就可以用这个迭代器函数访问容器内的数据，它在超出索引范围时直接返回：
```
if index >= len(i) {
			return
	}
```
否则返回i[index], true，即当前索引字符和true，以表示可以继续迭代。所以我们在main函数中只要检查这个返回值，当它返回false时中止迭代即可。见代码：
```
if !ok {
			break
	}
```


#### 4.如何理解闭包的迭代器
闭包的迭代器是比较抽象的知识，想要较好的理解它们，在我看来，唯有写代码一个途径。我来出一个题目，希望大家在看了本文后动手完成它。如果大家有兴趣，可以加入Go语言编程进阶群（537955063）。
题目：实现下面数据的迭代器
```
type  Strings []string
var s Strings={"a","b","c","d"}
```