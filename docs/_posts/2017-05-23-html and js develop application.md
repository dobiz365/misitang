---
layout: post
title: 用HTML5和JS开发本地应用程序
categories: javascript
description: 那么Javascript可以开发桌面程序吗？答案是肯定的。在Github上就有一个开源的项目node-webkit，现在已更名为nw.js。它就可以实现这个功能。那么它是怎样实现的呢？Nw.js这个项目采用webkit内核作为界面显示的引擎，众所周知，webkit是对html5支持最好的一个浏览器之一，我们可以用最新的html5技术来开发我们的桌面软件。但考虑到安全性，webkit不支持操作本地资源。Nw.js就想到了一个创新的想法，它把node.js集成了进来，而且让node.js和webkit的Javascript引擎运行在同一个进程中。
keywords: html,javascript,本地程序
---

## 用HTML5和JS开发本地应用程序

本人玩过很多计算机编程语言，从VB、C、VC++、DELPHI、JAVA一直到现在用的Javascript和node.js，可以用一句话概况“多而不精”，当然这也反映了计算机的发展过程，从单机程序向网络程序逐步过渡。但在我们的工作过程中会发现，在某些情况下，操作本地资源的能力也是不可或缺的，做一个桌面软件也很有必要。那么是不是需要重新拣起VC或DELPHI呢？不说需要重新安装几个G的开发环境，丢下用顺手的语言，重新用另一种语言，也很不合算。那么Javascript可以开发桌面程序吗？答案是肯定的。在Github上就有一个开源的项目node-webkit，现在已更名为nw.js。它就可以实现这个功能。那么它是怎样实现的呢？
Nw.js这个项目采用webkit内核作为界面显示的引擎，众所周知，webkit是对html5支持最好的一个浏览器之一，我们可以用最新的html5技术来开发我们的桌面软件。但考虑到安全性，webkit不支持操作本地资源。Nw.js就想到了一个创新的想法，它把node.js集成了进来，而且让node.js和webkit的Javascript引擎运行在同一个进程中。这样就完全打通了前端和后端。我们完全可以用html5和Javascript来开发一个桌面级的应用软件。下面我就以一个简单的功能开说明开发的经过。

首先要下载nw.js，可以在github下载源码自己编译。也可以到官网下载编译好的二进制文件或者是安装文件。在windows环境下，当然是下载msi安装文件更方便。下载安装后，有这样一目录结构：

![][1]

其中最重要的就是nw.exe这个可执行文件。我们只要编写好自己的脚本，然后把脚本拖到nw.exe上面即可运行，是不是非常简单呢？当然，我们也可以把我们的脚本打包为可执行文件。可惜，打包后的程序体积比较庞大，想想一下，我们的程序集成的功能，你就会明白体积庞大是必然的。

其次，编写我们的程序脚本。我们自己的脚本最好存放在一个单独的文件夹中。比如APP。在这个文件夹中，最重要的是package.json文件。这个文件定义了程序的入口文件，以及窗口的样式等等，

```javascript
{
  "name": "nw-demo",
  "version": "0.0.1",
  "main": "index.html",//入口的HTML文件
  "window": {                                                             
        "toolbar": true, //是否显示toolbar
        "width": 800, // 窗口的宽和高
        "height": 500,
        "frame": true                                
    }
}
```

这是一个JSON格式的文本文件，我们可以自己用记事本或notepad++来创建。
最后，就是编写我们自己的脚本了。其实就是写网页啊，作为前端工作人员，这肯定是您最擅长的工作了。当然，因为要操作本地资源，你还得会node.js。我要做一个把word复制过来的图文文档转换为HTML的软件。必须得把本地图片读取出来编码为BASE64符加在HTML中。好在html5本身支持剪贴板，我用如下代码为文本框绑定了onpaste（粘贴）事件：

```javascript
document.getElementById( 'testInput' ).addEventListener( 'paste', function( e ){
	var clipboardData = e.clipboardData;
	if(clipboardData.types.indexOf('text/html')!=-1){
		var s=clipboardData.getData('text/html');
		//将本地文件中的图片都转化为base64编码
		document.getElementById('wordhtml').innerHTML=s;
		var imgs=document.getElementsByTagName('img');
		for(var i=0;i<imgs.length;i++){
			//imgReader(imgs[i]);
			imgs[i].onload=imgReader(imgs[i]);
		}
	}
});
var imgReader = function( image ){
    var data=base64_encode(image.src);
	console.log(data);
	image.src=data;
};
```

关键代码在base64_encode中。这里需要调用本地资源，读取图片然后编码为base64格式。

```javascript
var fs = require('fs');
var os=require('os');
function base64_encode(file) {
	file=file.substr(8);//必须去除前面的file:///
	var head='data:image/png;base64,';
	var ext = file.substring(file.lastIndexOf('.') + 1).toLowerCase();
	console.log(ext);
	if(ext=='jpg' || ext=='jpeg'){
		head='data:image/jpg;base64,';
	}else if(ext=='png'){
		'data:image/png;base64,'
	}else if(ext=='gif'){
		'data:image/gif;base64,'
	}
    var bitmap = fs.readFileSync(file);
    // convert binary data to base64 encoded string
    var data= new Buffer(bitmap).toString('base64');
	return head+data;
}
```

相信对于熟练使用javascript的同学而言，这些代码是非常好理解的。
现在我们要去行这个程序，把APP这个文件夹往nw.exe上一拖，就开始运行了。
 
这里没有把toolbar去除，这是为了调试方便，在最终发行时可以很容易的去除，您在chrome上学到的调试手段，这里都可以用上。
把word文档粘贴上去后是这样的：

![](/images/posts/2.jpg)

这里的图片都是BASE64格式附加在HTML文档中了。这样的文档直储在数据库中虽然会拖慢数据库的响应，但管理起来着实方便。
可见这样的桌面程序开发方式实在是方便。希望本文能对您有所帮助。

[1]: /images/posts/1.jpg