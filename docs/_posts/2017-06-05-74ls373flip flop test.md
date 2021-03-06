---
layout: post
title: 74LS373三态输出的八D锁存器测试
categories: Electron
description: 74LS373是一种三态输出的八D锁存器集成电路，它的内部有8个锁存器，代表我们可以使用它存储八位二进制数据，在网上我们可以花1块多钱买到。它一共有20个引脚，其中8个输入数据引脚、8个输出数据引脚、2个电源引脚。重要的是另外这两个引脚，一个称为OE引脚，是集成电路的1号引脚，OE即是Output Enable的简称，它不对内部逻辑产生影响，只对输出有影响。
keywords: 74LS373,集成电路,测试
---

## 74LS373集成电路介绍

74LS373是一种三态输出的八D锁存器集成电路，它的内部有8个锁存器，代表我们可以使用它存储八位二进制数据，在网上我们可以花1块多钱买到。它一共有20个引脚，其中8个输入数据引脚、8个输出数据引脚、2个电源引脚。重要的是另外这两个引脚，一个称为OE引脚，是集成电路的1号引脚，OE即是Output Enable的简称，它不对内部逻辑产生影响，只对输出有影响。当OE为低电平时，输出可驱动电路，而当OE为高电平时，输出全部无效，不能驱动电路。如果需要改变内部锁存器的状态，需要用到LE引脚，当LE为高电平时，锁存器状态随输入改变，当LE为低电平时，D被锁存在已建立的数据电平。

## 74LS373集成电路引脚图

![](/images/posts/5.jpg)

## 74LS373集成电路测试电路

为了测试74LS373集成电路，我搭建了一个很简单的测试电路。详见图如下：

![](/images/posts/4.jpg)

功能连接图如下：

![](/images/posts/6.jpg)

## 总结
74LS373集成电路非常容易使用，它是电平控制的锁存器电路。经过检测发现输出高电平大约在3.7V左右，输出低电平大约在0.3V左右。如果我们要用分立元件搭建一个8位的锁存器电路，我想一定得用一个很大的电路板才能装的下，感谢集成电路的发明，让电子科技的发展日新月异。