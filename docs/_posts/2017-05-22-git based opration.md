---
layout: post
title: git基本操作列表
categories: Blog
description: git是程序员的基本功之一，在没用它之前总会觉得它很高大上，操作一定很难。其实，它的使用真的很简单。只要掌握几个简单的命令就可以用的很好。当然，如果想深度使用，还是需要对GIT思想有较好的理解，以及对vim编辑器的熟练使用。下面我把最基本的命令列了一下。
keywords: git
---

## git基本操作列表

git是程序员的基本功之一，在没用它之前总会觉得它很高大上，操作一定很难。其实，它的使用真的很简单。只要掌握几个简单的命令就可以用的很好。当然，如果想深度使用，还是需要对GIT思想有较好的理解，以及对vim编辑器的熟练使用。下面我把最基本的命令列了一下。默认仓库地址：https://github.com/dobiz365/misitang.git，它就是本文在github上的git仓库。

### 1.git clone
远程操作的第一步，通常是从远程主机克隆一个版本库，这时就要用到git clone命令。
如：
```
//请把仓库地址替换成您自己的地址
$ git clone https://github.com/dobiz365/misitang.git
```
clone后，git程序会自动创建目录。尤其要注意的是https都是需要ssh认证的。您需要先创建一个ssh证书，然后把Public证书上传到github上。这样git程序才能正确连接并下载仓库。

### 2.git remote
为了方便使用，每个仓库可以指定一个别名。git remote命令就用于管理别名。
如：
```
git remote  //列出所有主机
git remote add misitang https://github.com/dobiz365/misitang.git   //添加一个仓库别名
git remote rm misitang //删除仓库别名
```

### 3.get pull
git pull命令的作用是，取回远程主机某个分支的更新，再与本地的指定分支合并。
```
命令格式：
git pull <远程主机名> <远程分支名>:<本地分支名>

git pull misitang master:master  //取回misitang仓库master分支的文件并与本地仓库合并
git pull misitang 	//如果远程分支是与当前分支合并，则冒号后面的部分可以省略
```

### 4.git push
git push命令用于将本地分支的更新，推送到远程仓库，格式与git pull完全一样。
```
git push misitang master:master   //将本地仓库的更新推送到远程仓库
```

### 4.git add
git add命令主要用于把我们要提交的文件的信息添加到索引库中。
```
git add index.html  //将index.html添加到推送名单中
git add -A 			//将所有更新文件添加到推送名单中
```

### 5.git commit
git commit命令提交当前工作空间的修改内容。注意，该命令并这会上传文件到远程仓库。还需要再用git push 上传文件。
```
git commit -m "modify"   //-m 表示提交说明，它是必须的。
```

### 最后
一般我们使用git还是很简单的，首先git clone一个仓库，然后git remote指定一个别名，接下来你编辑这些文件，等工作做好后，git add 将更新文件加入索引表，git commit提交更新。最后git push上传到远程仓库。就是这样的工作流程。