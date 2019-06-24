---
title: git 常用操作(一)
entitle: git-common-operation-1
categories:
  - coding
tags: 
  - git
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws3.sinaimg.cn/large/006tNbRwgy1fv7u6mcbffj30go090wej.jpg
date: 2018-07-13 16:24:54
keywords:
description: 如何撤销已提交的 commits？如何清除工作目录中所有没有 tracked 过的文件？
---

这里列出几个我经常使用的，但又不怎么容易记住的 git 操作。

## 撤销已提交的 commits

已经提交的 commits，但是又不想用了，应该如何撤销?

打开GIT SHELL，切换到Git工作目录下，使用命令

```
git reset HEAD~N
```
N 是你想回滚到第几个 Commit 之前。

HEAD~N 处也可以填写想要回滚的 Commint 的 hash 值。

原来的:

commit0 -> commit1 ->commit2 ->commit3(HEAD)

`git reset HEAD~2` 之后:

commit0 -> commit1(HEAD) ->commit2 ->commit3

reset命令有三个可选的参数，分别是 soft,hard 和 mixed(默认)。

### mixed(默认）

--mixed 是 reset 在不指定任何参数时默认执行的操作。它将重置 HEAD 到另外一个commit，并且重置暂存区（也就是说暂存区里的内容全部清空了），但是也到此为止。工作目录不会被更改（即本地代码不会有任何改变）。所有该分支上从原 HEAD（commit）到你重置到的那个commit之间的所有变更将作为 local modifications 保存在工作区中，（被标示为local modification or untracked via git status)，但是并未存放到暂存区，你可以重新检查然后再做修改，提交到暂存区和commit。

commit0 -> commit1(HEAD) ->commit2 ->commit3

暂存区： 重置

工作目录：与commit3对应

按照上面的操作撤销已提交的 commits 之后，工作目录仍然没有变化，如果要撤销工作目录中的修改内容（也就是删除 commit1 到 commit3 期间修改的所有代码），可以执行以下操作
```
git checkout .  //撤销从上次提交之后所做的所有修改，注意命令之后有一个点
git checkout -- filepathname //撤销从上次提交之后指定文件的修改
```

### soft

--soft 参数告诉 Git 重置 HEAD 到另外一个 commit。仓库中的 HEAD 将停止在那里而其他什么也不会发生变化。这意味着暂存区、工作目录都不会做任何改变，所有的在原 HEAD 和你重置到的那个 commit 期间修改的代码在缓存(index)区域中。

比如，回退到 commits1，那么仓库中的代码会变成commit1，缓存区放置的是所有的在原 HEAD 和你重置到的那个 commit 之间的所有变更集，工作目录不会有任何变化。

commit0 -> commit1(HEAD) ->commit2 ->commit3

暂存区： 所有与从 commit1 到 commit3之间的变更集

工作目录：与commit3对应

总的来说，soft 和 mixed 的区别在于，soft指令 会把期间修改的代码放在暂存区中，而 mixed 指令不会。

### hard

--hard 参数将会彻底删除你在两个 commit 之间修改的代码。它将重置 HEAD 返回到另外一个commit，此时工作区会重置到对应的 commit 的状态，暂存区也已重置。这是一个比较危险的动作，具有破坏性，修改的代码会因此而丢失！
如果真是发生了数据丢失又希望找回来，那么只有使用 `git reflog` 命令了。

commit0 -> commit1(HEAD) ->commit2 ->commit3

暂存区： 重置

工作目录：与commit1对应

这里有一个特殊操作。如果我们希望彻底丢掉本地修改但是又不希望更改当前分支所指向的commit，则执行
`git reset --hard HEAD`
它将不会更改 commit，但是重置了暂存区，并且工作目录与 commit 对应。



## 如何清除工作目录中所有的没有 tracked 过的文件

git clean 命令用来从你的工作目录中删除所有没有 tracked 过的文件。

git clean 经常和 git reset --hard一起结合使用。**reset 只影响被 track 过的文件, 所以需要 clean 来删除没有 track 过的文件**。结合使用这两个命令能让你的工作目录完全回到一个指定的 commit 的状态

用法：
```
git clean -n
```
这是一次 clean 的演习, 告诉你哪些文件会被删除。他不会真正的删除文件，只是一个提醒。
```
git clean -f　　
```
作用是删除当前目录下所有没有 track 过的文件。他不会删除.gitignore文件里面指定的文件夹和文件，不管这些文件有没有被 track 过。
```
git clean -f <path>
```
作用是删除指定路径下的没有被 track 过的文件。
```
git clean -df
```
作用是删除当前目录下没有被 track 过的文件和文件夹
```
git clean -xf
```
作用是删除当前目录下所有没有 track 过的文件. 不管他是否是 .gitignore 文件里面指定的文件夹和文件

`git reset --hard` 和 `git clean -f` 是一对好基友。结合使用他们能让你的工作目录完全回退到最近一次 commit 的时候

git clean 对于刚编译过的项目也非常有用.如, 他能轻易删除掉编译后生成的.o和.exe等文件. 这个在打包要发布一个 release 的时候非常有用

下面的例子表示要删除所有工作目录下面的修改, 包括新添加的文件。假设你已经修改了一些代码，并且提交到缓存区了，但是你不需要这些代码了，你可以这么做re
```
git reset --hard
git clean -df
```
运行后，工作目录和缓存区回到最近一次 commit 时候一摸一样的状态，git status 会告诉你这是一个干净的工作目录，又是一个新的开始了！
