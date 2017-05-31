---
layout: post
title: 30分钟用Cordova制作蓝牙遥控小车APP
categories: Cordova
description: Cordova是一个用基于HTML、CSS和JavaScript的，用于创建跨平台移动应用程序的快速开发平台。它使开发者能够利用网页技术（HTML+CSS+JAVASCRIPT）来制作手机APP（iPhone、Android、Palm、Symbian、WP7、Bada和Blackberry等智能手机），而且我们在APP中可以轻松利用手机上的的核心功能——包括地理定位、加速器、联系人、声音和振动等，此外Cordova拥有丰富的插件，可以调用。
keywords: Cordova, Bluetooth, App, remote
---

## 30分钟用Cordova制作蓝牙遥控小车APP

### 1.背景
现在智能制造、创客等概念非常流行，这当然是由于软硬件发达到一定程度，让普通人也能享受到智能控制的乐趣。尤其是Arduino、树莓派等开源硬件的兴起，更是把这一活动推向了高潮。当然这些智能设备少不了要和手机通讯，所以自己做一个用手机蓝牙遥控的小车，会让你能对这里面的一些技术细节有个全面的了解。整个工程中肯定少不了一个手机APP，我们要用它来遥控小车。对于很多人来说，觉得制作一个手机APP是一件遥不可及的事，其实就现在来说，使用快速生成框架，制作一个比较简单的手机APP是分分钟的事。今天，我就带大家用Cordova平台来制作一个蓝牙APP，让大家领略一下Cordova的风采。

### 2.Cordova介绍
Cordova是一个用基于HTML、CSS和JavaScript的，用于创建跨平台移动应用程序的快速开发平台。它使开发者能够利用网页技术（HTML+CSS+JAVASCRIPT）来制作手机APP（iPhone、Android、Palm、Symbian、WP7、Bada和Blackberry等智能手机），而且我们在APP中可以轻松利用手机上的的核心功能——包括地理定位、加速器、联系人、声音和振动等，此外Cordova拥有丰富的插件，可以调用。
Cordova的主要优势是快速开发和跨平台，我们在Cordova中开发的APP，不用修改一行代码就可以部署到所有的手机上，这是何等方便的一件事啊。你不用学JAVA，不用学OBJECT-C，或者其它杂七杂八的知识，你只要会网页编程，那么你就可以轻松制作自己的APP。本文是Cordova深入的一篇文章，对于如何安装Cordova，请自己看相关博文，有英文基础的推荐官方网站[http://cordova.apache.org/](http://cordova.apache.org/)，在官网中不仅有如何安装，还有非常重要的API列表，更有我们下面要用到的插件下载（plugin 管理）

### 3.起步
使用命令行工具生成项目，逐条输入下面的命令生成项目：
```
cordova create bluecar com.tmnet.bluecar BlueCar
cd bluecar
cordova platform add android
cordova plugin add cordova-plugin-bluetooth-serial
```
打开config.xml增加一项，让APP横屏显示
```
<preference name="Orientation" value="landscape" />
```
然后连上手机，在手机上打开USB调试选项，再在命令行中输入以下命令，就可以看到一个APP被安装到手机上了。
```
cordova run android –device
```
  
### 4.界面和代码组织
现代手机都是支持多点触控的，我们的小车当然也要这么高大上，左边区域是速度和前后触摸区，右边区域是左右转向触摸区，界面如下：
！[](ui.png)
是不是非常简洁呢？
Cordova的界面都是HTML文件，只要您会网页设计，就可以设计APP。上面的UI可以用以下HTML结构来实现：
```
<div id='box_div'></div>
<div id='setting_btn'>蓝牙设备：（未连接）</div>
<div id='setting'></div>
```
box_div用于处理手指触摸，setting_btn就是右上角连接按钮，setting是点击按钮后弹出的蓝牙设备列表。如何用CSS让页面更好看，这就要看您的网页设计功力了。关键的技术点主要有两个：列出所有蓝牙设备和向蓝牙设备发送数据。
  
### 5.蓝牙连接
因为用到了cordova的插件，所以我们必须认真阅读这个插件的API文档。网址是：[https://www.npmjs.com/package/cordova-plugin-bluetooth-serial](https://www.npmjs.com/package/cordova-plugin-bluetooth-serial)
尤其是我们需要用到的几个API，它们主要有isEnable、list、connect、write这四个方法。方法签名如下：
```
bluetoothSerial.isEnabled(success, failure);
参数
success: 蓝牙设备开启时的回调函数
failure: 蓝牙设备关闭时的回调函数
```
要注意的是它使用的是回调的方式，我们不能立即、直接得到蓝牙的状态。需要在success这个回调函数是才能确定蓝牙开启了，只有在蓝牙开启的情况下我们才能接着进行下一步。
```
bluetoothSerial.list(success, failure);
success时返回的参数如下：
[{
    "class": 276,
    "id": "10:BF:48:CB:00:00",
    "address": "10:BF:48:CB:00:00",
    "name": "Nexus 7"
}, {
    "class": 7936,
    "id": "00:06:66:4D:00:00",
    "address": "00:06:66:4D:00:00",
    "name": "RN42"
}]
```
该方法返回的是一个数组，数组中的每个元素有四个字段，其中address和name字段是我们需要用到的，name字段用在界面显示，address用于和这个蓝牙设备连接。
```
bluetoothSerial.connect(macAddress_or_uuid, connectSuccess, connectFailure);
参数
macAddress_or_uuid: 蓝牙设备的address(在ios中是uuid)
connectSuccess: 连接成功是回调这个函数
connectFailure: 连接失败或关闭连接时回调这个函数

bluetoothSerial.write(data, success, failure);
参数
data: 要发送的数据（ArrayBuffer或是string）
success: 发送成功时回调
failure: 发送失败时回调
```
详细的API文档请参考上面的网址，里面的英文其实还是蛮简单的。

### 6.多点触摸处理
只要你的手机支持多点触措，那么Cordova也是支持多点触摸的。在Javascript实现规范中，对多点触摸的编程非常简单。首先需要绑定触摸事件
```
$('#box_div').on('touchstart',touchMove);  
$('#box_div').on('touchmove',touchMove);  
$('#box_div').on('touchend',touchEnd); 
```
原谅我还在使用jquery，因为它用着还挺方便的，也没碍着我什么，那为什么不用呢？
在touchMove中就是处理触摸事件的主要代码。在代码中首先取出触摸信息
```
var touches=evt.originalEvent.touches;
```
这个touches里面包含有当前手机上的所有触摸点，看你手机的支持情况了，说不定会有10个呢？它是一个数组，我们可以用如下循环来处理所有点的信息
```
for(var i=0;i<touches.length;i++){
	var touch=touches[i];
	//处理代码
}
```
每一个点中包含有很多信息
```
clientX / clientY: //触摸点相对浏览器窗口的位置
pageX / pageY: //触摸点相对于页面的位置
screenX / screenY: //触摸点相对于屏幕的位置
identifier: //touch对象的ID
target: //当前的DOM元素
```
有了这些信息我们就可以根据它们计算出当前触摸点应当发送的命令。下面以前进后退速度命令为例
```
//w,h触摸区宽和高  x,y触摸点的坐标
function speedMove(x,h,x,y){
	var arr=['5','4','3','2','1','0','6','7','8','9'];

	var idx;
	for(var i=0;i<10;i++){
		var y1=(h/10)*i;
		var y2=(h/10)*(i+1);		
		if(y>y1 && y<y2){
			idx=arr[i];
			break;
		}
	}
	if(connected) bluetoothSerial.write(idx);
}
```
我用0表示停止的命令，用1－5表示前进的速度等级，用6－9表示后退的速度等级。代码中我把触摸区划分为10个区，然后用一个循环来测试坐标点位于哪个区，然后输出相对应的命令即可。
  
### 7.总结
通过上面的讲解，我们制作这个APP的所有技术障碍已经没有了，需要的只是您的耐心和信心。当然30分钟只是噱头，它只对熟练掌握Cordova相关知识的人才有效，初学者还是忘记有这回事吧！
