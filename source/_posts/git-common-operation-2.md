---
title: git 常用操作(二)
entitle: git-common-operation
categories:
  - coding
tags:
  - javascript
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: /images/default-photos.jpg
date: 2018-09-13 09:04:38
keywords:
description:
---

## 删除的远程仓库，如何重新连接？
不小心删除了远程仓库的连接，用 git remote 查询之后结果为空，如何重新建立起连接？使用如下命令即可恢复。

```
git remote add origin https://github.com/username/YourRepository.git
```

中间的地址换成自己的代码仓库地址。
注意，此时本地仓库并没有追踪远程分支，使用 `git fetch origin` 命令重新追踪远程分支。
再使用 `git branch` 命令就可以查看到所有本地和远程的分支了。

## 将远程git仓库里的指定分支拉取到本地（本地不存在的分支）

通常，从远程仓库拉取分支到本地，操作方法是
```
git pull <远程主机名> <远程分支名>:<本地分支名>
```
拉取指定远程仓库指定分支到本地仓库指定分支,如不指定本地分支，默认是当前分支。

当我想从远程仓库里拉取一条本地不存在的分支时，应当使用如下命令：
```
git checkout -b <本地分支名> <远程主机名>/<远程分支名>
```
这个将会自动创建一个新的本地分支，并与指定的远程分支关联起来。

例如远程仓库里有个分支dev2,我本地没有该分支，我要把dev2拉到我本地：
```
git checkout -b dev2 origin/dev2
```
若成功，将会在本地创建新分支dev2,并自动切到dev2上。

如果出现提示：

fatal: Cannot update paths and switch to branch 'dev2' at the same time.
Did you intend to checkout 'origin/dev2' which can not be resolved as commit?

表示拉取不成功。我们需要先执行 `git fetch`，然后再执行
```
git checkout -b dev2 origin/dev2
```
此时本地新建的分支dev2与远程分支dev2保持一致。

## 如何保持 github 上 fork 的项目与原项目同步

fork 了别人的项目，一段时间之后，发现原项目更新了，如何把原项目的变更同步到自己 fork 的项目中，可以通过新建一个 upstream 远程主机来解决。操作方法如下。

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
git remote add upstream 原始项目仓库的 git 地址
```

如果继续使用 `git remove -v` 命令查看的话，就会发现这个时候该项目已经和原始的被fork的项目产生了关联。

第三步：保持 fork 之后的项目与原项目同步，使用如下的命令

```
git fetch upstream
```

之后，把原项目合并到自己 folk 之后的项目，比如合并到自己项目的 master 分支。
```
git merge upstream/master
```

如有冲突，需手动修改有冲突的代码。此时已经完成了 fork 项目与原项目同步，如果需要把项目代码推送到自己的远程仓库，就继续进行第四步。

第四步：push 本地项目代码到远程仓库，
 ```
 git push origin master
 ```
这样就完成了推送本地仓库的 master 分支到远程仓库（默认是与本地分支同名的远程分支)。
