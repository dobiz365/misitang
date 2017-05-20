---
layout: post
title: golang并发编程（2）
categories: golang
description:  上一篇讲了go并发编程的基本知识，本文带大家做了个简单的tcp并发服务器，并对它的性能做一个简单的测试。多线程服务器编程很难，今天就由米斯唐老师带给大家一个学习超简单的例子，带领大家走进服务器内部。
keywords: golang, 并发 ,tcp服务器
---

## golang并发编程（2）

上一篇讲了go并发编程的基本知识，本文带大家做了个简单的tcp并发服务器，并对它的性能做一个简单的测试。多线程服务器编程很难，今天就由米斯唐老师（[http://www.misitang.com](http://www.misitang.com)）带给大家一个学习超简单的例子，带领大家走进服务器内部。

#### 1.编写tcp服务器
编写一个多线程的tcp服务器至少需要两个函数，一个是main函数，一个是服务函数，在main函数中让程序在指定端口上监听用户的TCP连接，然后启一个无限循环，每一次有一个用户连进来，就开启一个协程，在这个协程中让用户和服务器交互。可见最简单的TCP服务器的代码相当简单。
```
package main

import (
	"bufio"
	"fmt"
	"net"
)

func main() {
	service := ":23"
	tcp, err := net.ResolveTCPAddr("tcp4", service)
	if err != nil {
		panic(err)
	}
	listen, err := net.ListenTCP("tcp", tcp)
	if err != nil {
		panic(err)
	}
	fmt.Println("Tcp listen to " + service)
	for {
		conn, err := listen.Accept()
		if err != nil {
			continue
		}
		go service_func(conn)
	}

}

func service_func(conn net.Conn) {
	reader := bufio.NewReader(conn)
	writer := bufio.NewWriter(conn)
	writer.WriteString("this is a tcp demo.\n")
	writer.Flush()
	for {
		s, _, err := reader.ReadLine()
		if err != nil {
			break
		}
		writer.WriteString(string(s) + "\n")
		writer.Flush()
	}
}
```
这个TCP服务和telnet服务还是很像的，它接收客户端的输入，当读入换行符时，把输入的字符串原样返回。每一个协程都启一个死循环，直到客户端退出为止。为了方便读写，使用了bufio中的Reader和Writer。

#### 2.编写测试客户端
我们需要在客户端模拟5000个用户的并发连接，它的程序结构和服务端很接近，同样需要main和函数，和并发处理函数。在main函数中建立连接，在client函数中处理数据。唯一不同的是，在建立所有5000个连接后，main函数需要等待所有协程处理完毕才能退出。这里我使用了chan int来等待，直到读取所有管道中的数据后主函数才退出。代码如下：
```
package main

import (
	"bufio"
	"fmt"
	"net"
	"runtime"
	"time"
)

func main() {
	runtime.GOMAXPROCS(runtime.NumCPU() - 1)
	service := "127.0.0.1:23"
	done := make(chan int)
	addr, err := net.ResolveTCPAddr("tcp4", service)
	if err != nil {
		panic(err)
	}
	clientNum := 5000
	succNum := 0
	for i := 0; i < clientNum; i++ {
		conn, err := net.DialTCP("tcp", nil, addr)
		if err != nil {
			fmt.Println("dial tcp error\n")
			continue
		}
		succNum++
		fmt.Println("dial tcp to service:" + service)
		go client(conn, done)
	}

	for i := 0; i < succNum; i++ {
		<-done
	}
}

func client(conn net.Conn, done chan int) {
	reader := bufio.NewReader(conn)
	writer := bufio.NewWriter(conn)
	reader.ReadLine()
	for i := 0; i < 600; i++ {
		s := fmt.Sprintf("is %d \n", i)
		writer.WriteString(s)
		writer.Flush()
		reader.ReadLine()
		time.Sleep(100 * time.Millisecond)
	}

	fmt.Println("one client done\n")
	done <- 1
}
```
注意处理函数中的time.Sleep(100 * time.Millisecond)，它表示发一个数据后等待100毫秒，如果一点不等，这么多数据可能会马上将输出缓冲区填满，从而导致TCP处理速度大大下降。在正常情况下，服务器和客户端一般是分离的，不会在一台机器上。
Golang是一种很有前途的计算机编程语言，如果大家有兴趣，可以加入Go语言编程进阶群（537955063）。

#### 3.结论
先打开服务端，然后打开客户端，我的机器CPU是E8400，4核 3.0G，内存2G，发现CPU占用大约70%左右，基本是3个核在用，1个核基本空着。内存占用基本没有发生变化。等待大约1分钟后，客户端所有协程相继退出，程序正常结束。
从中我们也可以看出，golang的并发真的非常强，编写也非常简单，如果只是实现一些简单的功能，相信就用一杯咖啡的功夫就能编写完成。实在是强大的生产力工具。就目前来看，我认为在当前电脑上，并发1万应当是没有问题的。如果需要编写这样的服务，我认为nodejs完全没有可比性。无论从数据结构的多样性、程序的健壮性……各方面完爆。在我眼中，nodejs还是比较适合写http服务。当然GO也不差，只是还没完全发展起来，模块数量比不上。