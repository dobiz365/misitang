---
layout: post
title: 使用vscode python进行第一个tensorflow程序实验
categories: Blog
description: vscode是一下跨平台的代码编辑软件，它是微软公司出品的免费良心软件。tensorflow是一个支持python开发的神经网络框架。现在把它们结合在一起，看看能起什么化学反映。
keywords: Ubuntu,迁移,经验
---

## 初步介绍
vscode是一下跨平台的代码编辑软件，它是微软公司出品的免费良心软件。tensorflow是一个支持python开发的神经网络框架。现在把它们结合在一起，看看能起什么化学反映。
我是看着《tensorflow 实战google深度学习框架》这本书来学习tensorflow的。

## vscode的一些问题
最简单的实验代码：
```
print('hello')
```
总的来说，vscode表现非常不错，主要不爽的是pylint老是提示我E1601错误，我认为它真的是不值一提，还好我不是完美主义者，就这样吧。

## 第一个tensorflow程序
```

import tensorflow as tf
#让tensorflow的一些提示信息不出现，您可以去掉下面两行实验一下。立即会出现一个提示，说可以开启哪个开关，让程序效率更高一些。
import os
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2' 

w1 = tf.Variable(tf.random_normal([2,3],stddev=2))
w2 = tf.Variable(tf.random_normal([3,1],stddev=2))

x=tf.placeholder(tf.float32,shape=(1,2),name="input")
a=tf.matmul(x,w1)
y=tf.matmul(a,w2)

sess = tf.Session()
#在当前版本的tensorfloww中，initialize_all_variable()函数已经废弃，改用下面这个函数，看起来tensorflow的开发比较频繁，API也经常改变。希望它越变越好吧。
init=tf.global_variables_initializer()
sess.run(init)

print(sess.run(y,feed_dict={x:[[0.7,0.9]]}))

```

## 代码解释

上面这么多行其实就实现了两个乘法，x是一个一维向量，w1是一个2行3列的向量，w2是一个3行1列的向量，y= x * w1 *w2。仅此而已。那为什么要使用tensorflow来实现这样简单的功能呢？主要是tensorflow把所有的计算进行了抽象，不仅可以普通PC上运行这样的乘法，还可以在GPU，甚至是在手机上运行。而且它的所有数据一经加载到tensorflow就无需再反复和python交换数据，这样就使它的运行速度不会被慢的一逼的python给拖累了。真正是一个天才的设计。


