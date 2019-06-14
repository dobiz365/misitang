---
layout: post
title: IPython Jupyter notebook使用详解
categories: Blog
description: 看到别人在浏览器上使用jupyter notebook学习python。感觉真的很酷。那么这个软件需要钱吗？安装麻烦吗？对于程序员来说，这一切都非常简单。使用以下几个命令即可安装完成。（当然，您需要先安装python，我推荐3.5以上版本）
keywords: Jupyter,python,notebook
---

一、安装Jupyter notebook。
看到别人在浏览器上使用jupyter notebook学习python。感觉真的很酷。那么这个软件需要钱吗？安装麻烦吗？对于程序员来说，这一切都非常简单。使用以下几个命令即可安装完成。（当然，您需要先安装python，我推荐3.5以上版本）

```bash
pip install jupyter
jupyter notebook
```

这时，就会弹出默认浏览器，并显示jupyter界面，新建一个项目，开始您的学习之旅吧！

二、修改默认字体和字号
方法还是比较多的，我推荐两种：1.是下载使用你喜欢的主题，当然也需要你要的字体和字号。2.修改jupyter自定义文件，它的位置在`/lib/site-packages/notebook/static/custom/custom.css`。然后加入下面这些内容即可：
```css
.CodeMirror {
    line-height: 1.4em;
    font-size: 16px;
    height: auto;
    background: none;
}
```
当然，CSS类名是在chrome浏览器的调试模式里找出来的。如果大家想改其它的样式，方法一样。