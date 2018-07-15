---
title: git 常用操作
entitle: ''
categories:
  - coding
tags: 
  - git
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: /images/default-photos.jpg
date: 2018-07-13 16:24:54
keywords:
description:
---

这里列出几个我经常使用的，但又不怎么容易记住的 git 操作。

## 1.撤销已提交的 commits

已经提交的commits，但是又不想用了，应该如何撤销。

打开GIT SHELL，切换到Git工作目录下，使用命令

```
git reset HEAD~N
```

N 是你想回滚到第几个 Commit 之前。

## 2.删除的远程仓库，如何重新连接？
不小心删除了远程仓库的连接，用 git remote 查询之后结果为空，如何重新建立起连接？使用如下命令即可恢复。

```
git remote add origin https://github.com/username/YourRepository.git
```

中间的地址可以换成自己的代码仓库地址。
注意，此时本地仓库并没有追踪远程分支，使用 `git fetch origin` 命令重新追踪远程分支。
再使用 `git branch` 命令就可以查看到所有本地和远程的分支了。


## 3.如何保持 github 上 fork 的项目与原项目同步

在网上搜索如何同步fork项目和原项目，很多热心的网友的各种解决方案。

1. 删除原有项目，在重新fork。(无疑这是暴力有效的，但绝不是我们想要的方式)

2. 在电脑本地同时 git clone 原项目和 fork 项目，用 git pull 更新，然后对比，再 git push 到 github上个人代码库。(虽有繁琐，却也是办法)

3. 本地 git clone 已 fork 的项目，然后添加远程库为原项目地址。(当然这里肯定要有项目访问权限的哦)

最后一种方法使用起来最为便捷，下面详细讲解该操作。

第一步：通过 github 的 web 页面 fork 目标项目,然后使用 git clone 命令在本地克隆自己 fork 的项目：

```
git clone https://github.com/YOUR-USERNAME/project—name
```

切换到此项目的路径下，使用如下命令

```
git remote -v
```

可以看到当前项目的远程仓库配置

```
github  https://github.com/YOUR-USERNAME/project—name.git (fetch)
github  https://github.com/YOUR-USERNAME/project—name.git (push)
```

第二步：复制被自己 fork 的原项目的 git 地址使用下面的命令：

```
git remote add upstream 原始项目仓库的git地址
```

如果继续使用 `git remove -v`命令查看的话，就会发现这个时候该项目已经和原始的被fork的项目产生了关联。

第三步：保持fork之后的项目与原项目同步，使用如下的命令

```
git fetch upstream
```

之后，把原项目和并到自己 folk 之后的项目，比如合并到自己项目的 master 分支。
```
git merge upstream/master
```

如有冲突，需手动修改有冲突的代码。此时已经完成了 fork 项目与原项目同步，如果需要把项目代码推送到自己的远程仓库，就继续进行第四步。

第四步：push 本地项目代码到远程仓库，
 ```
 git push origin master
 ```
推送本地仓库的 master 分支到远程仓库（默认是与本地分支同名的远程分支)
