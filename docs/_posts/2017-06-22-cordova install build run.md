---
layout: post
title: Cordova安装，编译第一个应用程序
categories: Blog
description: Cordova是Apache托管的一个开源多平台APP开发集成工具。它采用内嵌webview的方式来运行javascript代码，具有平台无关性。真正做到了一份代码在PC、IOS、ANDROID三个平台上不做任何修改的运行。虽然在流畅性方面逊色于原生代码开发，但是对于一般应用来说，现有机器的性能已经大大冗余。所以采用Cordova来开发普通应用就是非常好的选择。只要熟练掌握HTML+JS，你可以在2天内开发一个比较复杂的手机应用。这在以前来说真的不可想象。下面我就来介绍一下具体怎样做。
keywords: Cordova, Android, 搭建
---

## 介绍
Cordova是Apache托管的一个开源多平台APP开发集成工具。它采用内嵌webview的方式来运行javascript代码，具有平台无关性。真正做到了一份代码在PC、IOS、ANDROID三个平台上不做任何修改的运行。虽然在流畅性方面逊色于原生代码开发，但是对于一般应用来说，现有机器的性能已经大大冗余。所以采用Cordova来开发普通应用就是非常好的选择。只要熟练掌握HTML+JS，你可以在2天内开发一个比较复杂的手机应用。这在以前来说真的不可想象。下面我就来介绍一下具体怎样做。

## 开发环境搭建
### 1. 安装nodejs

Cordova需要依赖Nodejs以及它内部的npm，在Windows下，nodejs的安装非常简单，请大家自己下载nodejs的安装包进行安装。安装好nodejs后，需要在command环境下运行下面这两个命令，确认安装成功，如下图所示。

![](/images/posts/11.jpg)

### 2. 安装Cordova

安装好nodejs和npm后,就可以用npm命令来安装Cordova本身了。命令非常简单：
```bash
npm install -g cordova
```
现在我们就可以用Cordova来创建项目了。请大家参考[Cordova官方网站](http://cordova.apache.org)的教程来创建。基本的命令有：
```bash
cordova create demo com.tmnet.demo demo//创建项目，目录名demo 包名com.tmnet.demo 应用名demo
cd demo
cordova platform add android//添加安卓平台
cordova run android --device//在真机上运行项目
```

### 3. 安装jdk

在最后一步run的时候，Cordova会出一大堆的错误信息，如果大家懂英文的话，应该可以看出它需要JDK开发环境，安装很简单，去百度搜“jdk”，然后选择百度软件的下载就可以，选择它的原因就是下载速度真的很快，这样可以为我们节约好多时间。

![](/images/posts/12.jpg)

### 4. 安装Android sdk

由于我们添加了安卓平台的支持，而且需要生成APK包，所以Android sdk是必须的。Android 开发官方网站长期在墙外，对于普通开发者来说，还是挺麻烦的。可以在这个[地址](http://tools.android-studio.org/index.php/sdk)下载到安装包。它的下载速度非常快。
Android sdk还是挺大的，安装需要一些时间，安装好后运行sdk manager.exe，我们需要下载一些我们需要的SDK版本。默认它下载的是SDK26，而当前我在用的Cordova版本需要的是SDK25，所以大家多选几个版本吧。惊喜的是，SDK下载的速度真的很快。感谢有关部门没有把它也给封了。要知道在3年前，下载SDK可是生不如死啊。

### 5. 安装Gradle

因为Cordova需要用到Gradle来构建应用，所以它也是必须的，我在安装的时候看到的出错信息中显示，也可以安装Android studio,它内部就带有Gradle。但我试了一下，真还不行。本身对Android studio不熟悉，也不知道内部带的Gradle在哪个位置。在命令行下运行Gradle也不行。所以我它装了单独版本的Gradle4.0，下载解压，然后把bin添加到system path中即可。确认成功如下图：

![](/images/posts/13.jpg)

Gradle在低内存设备上会出一些问题，比如我的电脑只有2G内存，它在构建时就会出现“不能创建守护进程”这样的错误，在网上会找到各种各样的解决办法，对于cordova项目来说，靠谱的只有一个，就是修改GradleBuilder.js文件：
```javascript
原来：args.push('-Dorg.gradle.jvmargs=-Xmx2048m');//天哪，我的内存都被占完了
改为：args.push('-Dorg.gradle.jvmargs=-Xmx1024m');
```


### 6. 万事俱备，只欠东风
所有依赖组件都安装完毕，只要你连上手机，然后敲入`Cordova run android --device`，稍等片刻，应用就会被安装到你的手机上。

## 其它说明
有几个方面需要注意一下：
1. 手机要打开USB调试选项。
2. 一定要安装手机的驱动程序。比如我的小米note，没安装之前就无法调试，安装后就会多一个adb设备，就可供adb命令调试。

## 没了，现在尽情Rock Rock Rock吧。
