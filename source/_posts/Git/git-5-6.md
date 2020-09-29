---
title: Git学习笔记(二)版本管理
date: 2018-05-06 18:36:55
tags: 
- git 
- .gitignore 
- github
categories: Git学习笔记
description: Git版本管理系统，git reset，git checkout --file。
---

## Git版本控制系统
Git版本控制流程为：
工作区（Working Directory / Copy）->暂存区(stage / index)->分支。

![工作区、暂存区、分支（来自廖雪峰Git教程）](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTQyNDdkNjU3YTU3ODBmN2QucG5n?x-oss-process=image/format,png)


工作区即当前目录，其中有一个隐藏目录.git，为版本库，记录了所有版本信息，包含暂存区和分支等；

`$ git add`将文件从**工作区**添加到**暂存区**；

`$ git commit`将**暂存区**的内容提交到**分支**；

下面的名词解释十分精彩，引自：https://www.cnblogs.com/kidsitcn/p/4513297.html

> 首先我们来看几个术语
> - HEAD
> 这是当前分支版本顶端的别名，也就是在当前分支你最近的一个提交
> - Index
> index也被称为staging area，是指一整套即将被下一个提交的文件集合。他也是将成为HEAD的父亲的那个commit
> - Working Copy
> working copy代表你正在工作的那个文件集

> 当你第一次checkout一个分支，HEAD就指向当前分支的最近一个commit。在HEAD中的文件集（实际上他们从技术上不是文件，他们是blobs（一团），但是为了讨论的方便我们就简化认为他们就是一些文件）和在index中的文件集是相同的，在working copy的文件集和HEAD,INDEX中的文件集是完全相同的。所有三者(HEAD,INDEX(STAGING),WORKING COPY)都是相同的状态，GIT很happy。

> 当你对一个文件执行一次修改，Git感知到了这个修改，并且说：“嘿，文件已经变更了！你的working copy不再和index,head相同！”，随后GIT标记这个文件是修改过的。

> 然后，当你执行一个git add,它就stages the file in the index，并且GIT说：“嘿，OK，现在你的working copy和index区是相同的，但是他们和HEAD区是不同的！”

> 当你执行一个git commit,GIT就创建一个新的commit，随后HEAD就指向这个新的commit，而index,working copy的状态和HEAD就又完全匹配相同了，GIT又一次HAPPY了。

## 版本回退
HEAD是指向当前版本的指针。回退版本其实就是改变HEAD指向的位置：
![当前版本（来自廖雪峰Git教程）](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTU4YzI1YWNmOThhNmRlN2MucG5n?x-oss-process=image/format,png)

![回退一个版本（来自廖雪峰Git教程）](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy82MjQwNjY0LTkzNzliMGM5ZDMxYTM2MjcucG5n?x-oss-process=image/format,png)

版本回退命令为：
`git reset [--hard|soft|mixed|merge|keep] [<commit>或HEAD]`

其中，常用的选项分别有如下功能：
- hard：回滚 HEAD + index + 工作区，彻底没了；
- soft：回滚 HEAD，回到index，可以重新commit；
- mixed（默认参数）：回滚 HEAD + index，回到工作区，可以重新add。

### 回退前N个版本
上一个版本为HEAD\^，上上一个版本为HEAD^^，上n个版本为HEAD~n。

如果执行`$ git reset --hard HEAD`，什么都不会发生，因为还在原地。

回退一个版本：
`$ git reset --hard HEAD^`

回退10个版本：
`$ git reset --hard HEAD~10`

### 回退到指定版本
先`$ git log`查看版本库状态：
```
$ git log

commit 7afc632b7bbd7867fb8f74d313c0a558021645a3 (HEAD -> master, origin/master, origin/HEAD)
Author: zhang35 <zhangjqfriend@126.com>
Date:   Wed Jan 10 12:26:17 2018 +0800

    Update File.txt

commit 287ed58123b17ba814c2d94ad593c8cfc11af1a6
Author: zhang-pc <zhangjqfriend@126.com>
Date:   Wed Jan 10 12:25:52 2018 +0800

    fun

```

指定版本号前几位即可：

 `$ git reset --hard 287ed`

### 撤销回退
如果回退到某版本后后悔了，想返回到原先的版本，可以
`$ git reflog`查看命令记录：

```
$ git reflog

287ed58 (HEAD -> master) HEAD@{0}: reset: moving to 287ed
7afc632 (origin/master, origin/HEAD) HEAD@{1}: clone: from git@github.com:zhang35/Hello-Git.git

```

找到之前的版本id即可恢复回去：

 `$ git reset --hard 7afc`

## 丢弃修改
使用`$ git status`查看状态，可以看到如何撤销的提示。

### 丢弃暂存区的修改
对于已经add到暂存区，但未commit的修改，`$ git status`提示如下：
```
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   File.txt
```
移出暂存区：
`git reset HEAD File.txt `

（HEAD即当前最新版本）

这样对File.txt的修改就回到了工作区。
### 丢弃工作区的修改
对于尚在工作区，还没add到暂存区的修改，`$ git status`提示如下：
```
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   File.txt
```
从工作区丢弃更改：
`$ git checkout -- File.txt`

(命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令)

这时`$ git status`提示如下：
```
On branch master
nothing to commit, working tree clean
```
世界清净了。

## 小结
简介了Git版本管理系统，总结了版本回退、丢弃修改的基本方法。