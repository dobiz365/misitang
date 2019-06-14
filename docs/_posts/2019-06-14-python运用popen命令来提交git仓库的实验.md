---
layout: post
title: python运用popen命令来提交git仓库的实验
categories: [git,python,popen]
description: 本人的博客是部署在github上的免费博客，但是每一次撰写都是一个痛苦的体验。以至于慢慢的更新越来越少，这几天突然想到，为什么不写一个简单的程序来自动更新博客呢？
keywords: git,python,popen
---

# python运用popen命令来提交git仓库的实验

本人的博客是部署在github上的免费博客，但是每一次撰写都是一个痛苦的体验。以至于慢慢的更新越来越少，这几天突然想到，为什么不写一个简单的程序来自动更新博客呢？

我使用的技术平台是：`python`+`git`。我使用wxpython来构建gui界面，在构建界面的时候，我使用了`wxFormBuilder`这个免费软件。用它可以快速的创建自己的gui。具体的使用方法我这里就不介绍了，使用相当简单，功能也很单一。

软件中最关键的功能是要自己提交到git仓库。我使用了os.popen来运行cmd命令。由于需要运行多个cmd命令，查资料后发现这样处理即可：

1. 命令被分号“;”分隔，这些命令会顺序执行下去；
2. 命令被“&&”分隔，这些命令会顺序执行下去，遇到执行错误的命令停止；
3. 命令被双竖线“\|\|”分隔，这些命令会顺序执行下去，遇到执行成功的命令停止，后面的所有命令都将不会执行;

所以我将命令用&&连接，一个简单的软件就做好了。

```

cmd=config['dir'][0:2]+" && cd "+config['dir']+" && git add -A && git commit -m \"add\" && git push"
ret=os.popen(cmd,shell=True)

```
