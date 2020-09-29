---
title: Git学习笔记(一)基础用法
date: 2018-05-05 16:56:26
tags: 
- git 
- .gitignore 
- github
categories: Git学习笔记
description: Git简介，Git命令简写,.gitigore，git init、git add、git commit、git remote、git push。
---

## Git简介
Git是当前最流行的分布式版本控制系统。Git是一个本地软件，需要下载安装。

## 简化命令
为了方便使用，首先自定义简化指令：

`$ git config --global alias.st status`

更简单的做法是，在用户主目录下的隐藏文件.gitconfig 中加入：
```
[alias]  
    st = status  
    ci = commit  
    br = branch  
    co = checkout  
    df = diff  
    unstage = reset HEAD --
    last = log -1 HEAD
```
这样，git status/commit/branch/checkout等反人类的长命令，可以敲俩字母代替了。

## 新建版本库
在当前文件夹创建版本库：
```
$ mkdir leader-front
$ cd leader-front
$ git init
```

## 添加文件：
```
git add [--verbose | -v] [--dry-run | -n] [--force | -f] [--interactive | -i] [--patch | -p]
      [--edit | -e] [--[no-]all | --[no-]ignore-removal | [--update | -u]]
      [--intent-to-add | -N] [--refresh] [--ignore-errors] [--ignore-missing]
      [--chmod=(+|-)x] [--] [<pathspec>…​]
```

添加指定文件：
`$ git add file1.txt`
`$ git add file2.txt file3.txt`

添加多个文件：
`$ git add document/*.txt`

添加所有文件：
`$ git add .`

### .gitignore忽略文件
对于不想添加到仓库的文件，如Desktop.ini，可以通过写.gitignore文件忽略之。

[https://github.com/github/gitignore](https://github.com/github/gitignore)提供了常用的配置文件。

.gitignore例子：
```
# Windows:
Thumbs.db
Desktop.ini

# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build
```

把.gitignore文件单独提交到git后即生效。检验其是否生效方法是，`$ git status`命令是否显示`working directory clean`。

gitignore还可以指定要将哪些文件添加到版本管理中：
```
!*.zip
!/mtk/one.txt
```
唯一的区别就是规则开头多了一个感叹号，Git会将满足这类规则的文件添加到版本管理中。

## 提交修改：
```
git commit [-a | --interactive | --patch] [-s] [-v] [-u<mode>] [--amend]
       [--dry-run] [(-c | -C | --fixup | --squash) <commit>]
       [-F <file> | -m <msg>] [--reset-author] [--allow-empty]
       [--allow-empty-message] [--no-verify] [-e] [--author=<author>]
       [--date=<date>] [--cleanup=<mode>] [--[no-]status]
       [-i | -o] [-S[<keyid>]] [--] [<file>…​]
```

提交暂存区的改动：
`git commit -m "info"`

必须得有改动说明。如果没用-m指定，会单独开一个窗口让你写。

## 上传代码到远程仓库
通常使用Github等远程仓库存储、共享代码。

Github是Git的一个网络实现，提供了免费的远程代码存储仓库。

由于Github服务器在国外，访问速度感人，可以选用国内类似的代码仓库，比如：码云，coding.net等。

下面以Git+Github管理前端项目leader-front为例，总结如何使用远程仓库存储代码。

### 从本地上传代码到Github
#### 关联远程仓库
登录Github，创建仓库leader-front，全是鼠标操作。
*(在自己的Github账户中，需要添加本机SSH公钥后，才能推送)*

回到本地命令行，关联远程仓库：
```
$ git remote add origin git@github.com:zhang35/leader-front.git
```
名为origin（origin是git远程仓库的默认名称），以后直接`$ git push`即可上传。

也可以起名为“github”，以后使用`$ git push github`上传。

#### 上传代码
关联远程仓库后，直接输入`$ git push`后会提示：
```
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin master

```
因为此时只关联了仓库，还没关联分支。

不关联分支的情况下，执行：

`$ git push origin master`

即可将当前分支推送到远程origin下的master分支。

为了简化这一步骤，第一次推送前，根据提示添加默认关联分支：

` $ git push --set-upstream origin master`

或同时完成推送和关联：

`$ git push -u origin master`

加上-u参数，本地的master分支和远程的master分支就关联了起来。

如果出现如下错误：
```
error: src refspec master does not match any.
error: failed to push some refs to 'git@github.com:zhang35/leader-front.git'
```
则是因为此时本地仓库为空，需要添加、提交修改才能提交。

### 从Github获取代码
上一小节的做法是先有本地仓库，再与远程仓库关联。

更方便的做法是，先有远程仓库，从远程仓库克隆到本地，自动完成关联：

`$ git clone git@github.com:zhang35/leader-front.git`

或

`$ git clone https://github.com/zhang35/leader-front.git`

## 例子：将本地项目上传到Github管理的完整流程
```
$ mkdir leader-front
$ cd leader-front
$ git init
$ git add .
$ git ci -m 'add all'
$ git remote add origin git@github.com:zhang35/leader-front.git
$ git push -u origin master
```

## 小结
总结了Git的基本用法，以及使用远程仓库储存代码的基本方法。

