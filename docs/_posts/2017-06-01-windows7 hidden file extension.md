---
layout: post
title: windows7中显示“隐藏已知文件类型的扩展名”
categories: Blog
description: 比如有时候我想把文件扩展名给隐藏起来，但过了几天又想把扩展名给显示出来（是不是感觉很蛋疼啊，其实这样的操作还真存在，也不少见。）但打开资源管理器却发现，总是找不到“隐藏已知文件类型的扩展名”的选项，难道有人把它吃了吗？
keywords: windows7,隐藏扩展名
---

## windows7中显示“隐藏已知文件类型的扩展名”

## 问题产生
windows7总体来说是一个很不错的操作系统，但它也时不时的会发一点疯，比如有时候我想把文件扩展名给隐藏起来，但过了几天又想把扩展名给显示出来（是不是感觉很蛋疼啊，其实这样的操作还真存在，也不少见。）但打开资源管理器却发现，总是找不到“隐藏已知文件类型的扩展名”的选项，难道有人把它吃了吗？

![](/images/posts/3.jpg)


## 解决问题
想不通为什么会产生这样问题，于是就在百度上搜了一下，发现有这样问题的电脑还挺多，现成的解决办法一大把。但很多回答却很不靠谱，最终我使用以下办法解决。办法如下：
1. 找开记事本，把下面这个注册表信息复制进去。
2. 保存为.reg文件，一定要注意不要保存为.txt文件。
3. 关闭记事本，双击reg文件运行它，将注册表信息导入系统。
4. 完成。

```bash
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\explorer\Advanced\Folder\'HideFileExt]
"Type"="checkbox"
"Text"="@shell32.dll,-30503"
"HKeyRoot"=dword:80000001
"RegPath"="Software\\Microsoft\\Windows\\CurrentVersion\\Explorer\\Advanced"
"ValueName"="HideFileExt"
"CheckedValue"=dword:00000001
"UncheckedValue"=dword:00000000
"DefaultValue"=dword:00000001
"HelpID"="shell.hlp#51101"
```

## 总结
现在看来，出现这样问题的主要原因是注册表信息丢失，真不明白，好好的注册表信息为什么会丢了呢？是磁盘损坏、还是其它程序破坏。而且这样的解决办法，在我看来还是很不合格的。它一定是微软公司想出来的，普通用户真的很难学会呢！