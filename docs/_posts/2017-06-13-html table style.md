---
layout: post
title: 一种清爽的表格样式
categories: HTML
description: 对于非设计专业的程序员来说，设计一种漂亮的表格样式非常因难，不仅仅是因为CSS不太精通，也是因为对于设计实在是没有爱，所以一般来说都是在网上找一些现成的设计，然后改一下。能改已经非常不错了哦！
keywords: HTML , Table , CSS
---

## 展示
对于非设计专业的程序员来说，设计一种漂亮的表格样式非常因难，不仅仅是因为CSS不太精通，也是因为对于设计实在是没有爱，所以一般来说都是在网上找一些现成的设计，然后改一下。能改已经非常不错了哦！这个样式的最终展示结果如下：

![](/images/posts/10.jpg)

## 使用
样式的使用非常简单：可以把以下样式放在网页的style标签中，就可以使用。

1. 可以把以下样式放在网页的style标签中，就可以使用。
2. 也可以单独做一个CSS文件，然后通过link标签引入。

```css
table {
    *border-collapse: collapse; /* IE7 and lower */
    border-spacing: 0;
    width: 100%;    
}
.zebra td, .zebra th {
    padding: 10px;
    border-bottom: 1px solid #a2a2a2;
	color:#fff;    
}

.zebra tbody tr:nth-child(even) {
    background: #555;    
}

.zebra th {
	color:#000;
    text-align: left;
    text-shadow: 0 1px 0 rgba(255,255,255,.5); 
    border-bottom: 1px solid #ccc;
    background-color: #dce9f9;
    background-image: -webkit-gradient(linear, left top, left bottom, from(#ebf3fc), to(#dce9f9));
    background-image: -webkit-linear-gradient(top, #ebf3fc, #dce9f9);
    background-image:    -moz-linear-gradient(top, #ebf3fc, #dce9f9);
    background-image:     -ms-linear-gradient(top, #ebf3fc, #dce9f9);
    background-image:      -o-linear-gradient(top, #ebf3fc, #dce9f9); 
    background-image:         linear-gradient(top, #ebf3fc, #dce9f9);
}

.zebra th:first-child {
    -moz-border-radius: 6px 0 0 0;
    -webkit-border-radius: 6px 0 0 0;
    border-radius: 6px 0 0 0;  
}

.zebra th:last-child {
    -moz-border-radius: 0 6px 0 0;
    -webkit-border-radius: 0 6px 0 0;
    border-radius: 0 6px 0 0;
}

.zebra th:only-child{
    -moz-border-radius: 6px 6px 0 0;
    -webkit-border-radius: 6px 6px 0 0;
    border-radius: 6px 6px 0 0;
}

.zebra tfoot td {
    border-bottom: 0;
    border-top: 1px solid #fff;
    background-color: #f1f1f1;  
}

.zebra tfoot td:first-child {
    -moz-border-radius: 0 0 0 6px;
    -webkit-border-radius: 0 0 0 6px;
    border-radius: 0 0 0 6px;
}

.zebra tfoot td:last-child {
    -moz-border-radius: 0 0 6px 0;
    -webkit-border-radius: 0 0 6px 0;
    border-radius: 0 0 6px 0;
}

.zebra tfoot td:only-child{
    -moz-border-radius: 0 0 6px 6px;
    -webkit-border-radius: 0 0 6px 6px
    border-radius: 0 0 6px 6px
}
```

## DEMO
可以通过下面的HTML代码来体验表格样式。

```html
<table class='zebra' >
<tr><th>用户名</th><th>出生日期</th></tr>
<tr><td>张三</td><td>2017-06-12</td></tr>
<tr><td>李四</td><td>2017-06-23</td></tr>
</table>
```