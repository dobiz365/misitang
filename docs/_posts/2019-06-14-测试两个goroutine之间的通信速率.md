---
layout: post
title: 测试两个goroutine之间的通信速率
categories: [golang]
description: 测试两个goroutine每秒可以发送多少信息
keywords: golang,goroutine,chan
---

// test.go
package main

import (
	"fmt"
	"time"
)

/*
测试两个goroutine每秒可以发送多少信息，在没有缓冲的情况下，大约为1615934，机器CPU E8400，内存2G
在有缓冲时，如缓冲为10，大约为4353553
可见缓冲可以显著提高吞吐量
*/
func main() {
	chan1 := make(chan int, 10)
	go func() {
		var i int = 0
		var t1, t2 int64
		t1 = time.Now().UnixNano()
		for {
			chan1 <- i
			i++
			t2 = time.Now().UnixNano()
			if t2-t1 > 1e9 {
				fmt.Println(i)
				close(chan1)
				return
			}
		}
	}()
	go func() {
		var i int
		for {
			for i = range chan1 {

			}
		}
		fmt.Println(i)
	}()
	<-time.After(2 * time.Second)
}
