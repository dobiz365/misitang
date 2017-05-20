---
layout: post
title: Go语言实现迭代器之最简实现
categories: golang
description: 所谓迭代器就是可以遍历容器的一个接口，实现迭代器有很多方法。今天米斯唐老师给大家带来一个最简单的迭代器。
keywords: golang, 迭代器
---

Go语言实现迭代器之最简实现

所谓迭代器就是可以遍历容器的一个接口，实现迭代器有很多方法。今天米斯唐老师（[http://www.misitang.com](http://www.misitang.com)）给大家带来一个最简单的迭代器。

#### 1.迭代器实现原理
golang是最新的计算机编程语言，它实现了最新的一些编程思想，比如把函数当作一种数据类型，比如匿名函数……使用这些现代编程语言的特性，我们可以很容易的做出一个迭代器。加入go语言编程进阶群（537955063），大家一起交流编程知识。不多说了，请看代码：
```
package main

import "fmt"

type Ints []int

func (i Ints) Do(fn func(int)) {
	for _, v := range i {
		fn(v)
	}
}

func main() {
	ints := Ints{1, 2, 3}
	ints.Do(func(v int) {
		fmt.Println(v)
	})
}
```

#### 2.代码解读
提高编程水平第一靠写代码，第二靠读代码，两者缺一不可。上面代码的关键是Do这个函数：
```
func (i Ints) Do(fn func(int)) {
```
表示它是Ints的一个实现，在Ints上调用时，会把Ints实例变量发送到函数中，它接收一个参数，参数的类型是func(int)，也就是说是一个函数。这样就可以在函数内部引用i和fn了。使用for range来循环遍历slice，每遍历一个元素就调用fn函数，并把值传过去。

#### 3.理解要点
这段代码中最难理解的是匿名函数的使用、函数作为参数传递这两点。对于它们，您需要多多练习，之后你就可以把函数当作一个普通类型来看待，它和整型变量、字符串等也并没什么多大分别。你大可以把函数看作一个变量，而这个变量还可以执行，真是个特别的变量。
对于这些功能，Javascript使用的最多，大家可以参考Javascript的书籍来理解，我就是这样走过来的。