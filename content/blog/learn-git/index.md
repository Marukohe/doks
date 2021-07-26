---
title: "Learn Git"
description: ""
lead: ""
date: 2021-03-31T22:49:13+08:00
lastmod: 2021-03-31T22:49:13+08:00
draft: false
weight: 50
images: []
contributors: [He Wei]
---

## 创建版本库

### 初始化

```bash
git init
```

## 添加文件

```bash
git add xxx
git commit -m "xxx"
```

## 时空穿梭机

`git status`命令可以查看当前仓库的状态,`git diff`可以查看具体修改了什么内容.

### 版本回退

`git log`可以显示从最近到最远的提交记录,如果感觉信息太多,可以使用`git log --pretty=oneline`命令.

`git reset --hard HEAD^`回退到上一个版本,也可以使用`git reset --hard commit_id`

`git reflog`查看命令历史,可以查看需要返回的版本.

### 工作区和暂存区

### 管理修改

### 撤销修改

`git checkout -- filename`命令可以撤销上一次修改

`git reset HEAD filename`可以将暂存区的内容撤销.

### 删除文件

`git rm filename`用于删除一个文件

`git checkout -- filename`用于回复一个误删的文件.(不过为什么没有成功恢复...)

可以用`git reset --hard commit_id`恢复至需要的版本

## 远程仓库

```bash
 ssh-keygen -t rsa -C "youremail@example.com"
```

用于生成公钥和私钥

之后在`github`的SSH设置中将`id_rsa.pub`的内容粘贴上去

### 添加远程库

在`github`上创建新的`Repository`

在本地仓库运行

```bash
git remote add origin git@github.com:Yourname/learngit.git
```

之后可以通过`git push -u origin master`命令将本地库中的内容推到远程库

`-u`命令可以把本地的`master`分支和远程的`master`分支关联起来.

之后只要通过`git push origin master`命令便可以将本地`master`分支的内容推送到远程仓库中.

### 从远程仓库克隆

使用`git clone xxx`命令

## 分支管理

### 创建与合并分支

查看分支:`git branch`

创建分支:`git branch branchname`

切换分支:`git checkout branchname`

创建+切换分支:`git checkout -b branchname`

合并某分支到当前分支:`git merge branchname`

删除分支:`git branch -d branchname`

### 解决冲突

同时在其他分支和主分支修改内容可能会造成`merge`冲突,需要再次修改保存内容,之后可以合并分支.

通过`git log --graph`命令查看分支图.

### 分支管理策略

通常,合并分支时,`Git`会用`Fast forward`模式,删除分支之后,会丢掉分支信息.

可以通过`--no-ff`命令禁止`Fast forward`模式,之后用`git log`查看记录可以发现分支`dev`信息保留
{{< img src="15.png" alt=Rectangle" caption="--no-ff" class="border-0" >}}

### Bug分支

有时候需要在其他工作区修复`bug`,通过`git stash`命令保存当前工作区.

之后通过`git stash list`命令查看保存的内容,`git stash apply`命令恢复,通过`git stash drop`删除`stash`中的内容.

也可以通过`git stash pop`命令恢复并删除.

### Feature分支

通过`git branch -D branchname`命令删除修改之后未合并的分支

### 多人协作

`git remote`查看远程库信息

`git remote -v`显示详细信息

`git clone`抓取远程库信息

`git pull`讲最新提交从远程库抓取下来

`git pull --set-upstream-to=origin/dev dev`将本地`dev`分支与远程分支链接

### Rebase

## 标签管理

### 创建标签

`git tag name`命令可以创建一个标签,与当前最新的`commit`关联

`git tag`命令查看所有标签

`git tag name commit_id`可以将标签加到某个指定的`commit`

`git show tagname`查看标签信息

`git tag -a tagname -m "description" commit_id`为某个`commit`加上标签并描述

### 操作标签

`git push origin tagname`可以推送一个标签到远程

`git push origin --tags`推送所有未推送的标签到远程

`git tag -d tagname`可以删除一个本地标签

`git push origin :refs/tags/tagname`删除一个远程标签

## 自定义git

### 忽略特殊文件

添加`.gitignore`文件

### 配置别名

```bash
git config --global alias.xxx 'xxxxx'
```

参考网站:  [廖雪峰git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
