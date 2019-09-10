---
title: git同步
date: 2019-09-09 21:33:33
tags: git
categories: git
---
# hexo多电脑同步
参考[这里](https://www.jianshu.com/p/fceaf373d797)
1、先在github仓库yourname.github.io上创建分支hexo并将其设置为主分支，保证git add和commit提交到hexo中，并且需要清空hexo的内容，需要先克隆再删除除.git文件后提交`push`。
2、将本地的hexo项目上传到github hexo分支上`git add .`和`git commit -m "move"`，先git init创建本地仓库，并与远程仓库连接。。好像clone之后就会与远程连接
3、然后生成ssh key并添加到github设置中，将hexo分支上的内容克隆到本地即可，记得每次修改配置内容之后都`git pull origin hexo`来同步github仓库上的内容。

## 遇到的问题
1、上传时要先将themes中的.git文件删除，如果先上传再删除的话，需要先清空一下git中的缓存`git rm -rf -- cached 你想清除缓存的目录`，再使用`git add -A`上传修改后的部分即可。