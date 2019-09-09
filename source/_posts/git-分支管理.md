---
title: git 分支管理
date: 2019-07-31 17:48:24
tags: other
categories: git
---
# git分支管理
master是默认创建的主分支，HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。
一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点，提交时每次会提交给HEAD指向的当前分支。
但是为了保证更加安全，可以通过创建新分支，提交到新的分支，再将其与主分支合并。
参考[这里](https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424)
1、分支创建

```
git branch dev
```
2、改变提交的分支，即用HEAD指向指定的分支，即以后提交就会提交到现在指定的分支上

```
git checkout dev
```
3、创建新分支，并将HEAD指向当前分支

```
git checkout -b dev
```
4、查看所有的分支

```
git branch
```

## 简单的无冲突的分支合并
5、合并分支，当前HEAD指向master执行合并操作之后，会将master直接指向dev

```
git merge dev
```
6、删除分支，当合并之后可以删除当前分支

```
git branch -d dev
```
## 有冲突的分支合并
当两个分支均出现了修改，就需要先统一修改之后，提交，然后删除多余分支
通过`git status`查看当前状态，以及分支状态
使用`git log --graph --pretty=oneline --abbrev-commit`来查看当前的分支状态

```
git log --graph
```
例子：这是两个branch同时修改一个文件，可以查看冲突的文件，上面是当前分支修改的，下面是new这个分支修改的
```
this is a test
modify
<<<<<<< HEAD
this is an apple
=======
this is 一个 apple
>>>>>>> new
```
##  分支管理
通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息，可以看出合并过的痕迹。
```
git merge --no-ff -m "merge with no-ff" dev
```
分支策略
在实际开发中，master主分支应该是稳定的，每次修改在分支上修改，然后不断合并分支，最后统一后合并在master分支上
## bug管理
当你想修复另外的一个bug时，需要先放下当前的工作的工作，
1、保存现场
```
git stash
```
2、切换分支，bug修改好提交
3、恢复现场
查看保存的现场列表
```
git stash list
```
恢复现场、但是不删除列表中的值
```
git stash apply
```
恢复现场并删除列表中的值
```
git stash pop
```
## 丢弃分支

```
git branch -d dev
```
如果分支还没有被合并就丢弃，需要强制删除通过

```
git branch -D dev
```
## 多人合作，远程推送
当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，**远程仓库的默认名称是origin**。
这一块好像和之前往github上传文章一样，比之前的详细
1、查看远程仓库：

```
git remote
```
2、查看远程仓库更详细的信息：

```
git remote -v
```
3、远程推送分支
推送主分支
```
git push origin master
```
推送其他分支

```
git push origin dev
```
4、从远程库克隆仓库

```
git clone git@github.com:michaelliao/learngit.git
```
5、多人推送，导致冲突时，远程仓库的内容多时，通过`git pull`将仓库内容与本地内容同步，需要指出需要同步的远程分支origin=dev和本地分支dev

```
git branch --set-upstream-to=origin/dev dev
```
参考[这里](https://www.liaoxuefeng.com/wiki/896043488029600/900375748016320)
## rebase 分支整理
使用`git rebase`
rebase操作可以把本地未push的分叉提交历史整理成直线；
rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。