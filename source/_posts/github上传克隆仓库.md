---
title: github上传克隆仓库
date: 2019-07-31 15:47:56
tags: other
categories: git
---
# 向github上传文件
首先远程需要设置一下ssh密钥
参考[这里](https://www.cnblogs.com/salmonlin/p/7805409.html)
## 上传
（1）远程仓库
```
$ git remote add origin git@github.com:yourName/yourRepo.gitwison。
```
yourName是你的用户名，yourRepo.gitwison是你要上传项目的仓库。
(2)提交项目
提交时会把仓库中HRED指向的版本进行提交，所以需要先把想要提交的项目commit上去
`git push origin master`，第一次可以通过`git push -u origin master`可以在GitHub查看项目，当提交的仓库非空时，或者仓库两边没有同步，先使用`git pull --rebase origin master`同步两边
(3)更新项目，先添加文件，再提交，更新前最好用`git pull origin master`更新一下你的本地项目，因为可能有别人做了更新
## 克隆
克隆有两种方式，一种通过ssh`git@github.com:condyboy/a.git`，一种通过https`https://github.com/condyboy/a.git`
使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https。
通过`git clone https://github.com/condyboy/a.git`这里使用https为例

