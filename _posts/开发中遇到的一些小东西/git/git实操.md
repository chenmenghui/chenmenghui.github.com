---
title: git实操
date: 2020-3-29
categories: git
tags: git
---

### 简单命令行入门教程

Git 全局设置:

```
git config --global user.name "陈孟辉"
git config --global user.email "chenmenghui_008@163.com"
```

创建 git 仓库:

```
mkdir chenmenghui
cd chenmenghui
git init
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin git@gitee.com:chenmenghui/chenmenghui.git
git push -u origin master
```

已有仓库?

```
cd existing_git_repo
git remote add origin git@gitee.com:chenmenghui/chenmenghui.git
git push -u origin master
```

### 迁移仓库

前几日把在自己的服务器上建了一个git裸仓库。然后想着自己的服务器可能用不了多久，就想着把项目迁到码云上来。

首先使用

```cmd
git clone --bare 「自己的服务器上的仓库」
git push --remote 「码云上刚刚创建的仓库」
```

### clone代码之后,把项目中的.env配置文件修改之后,即使将其加入了.gitignore文件,也被提交了

使用

```
git update-index --assume-unchanged .env
```

才没提交

[原来如此](https://www.cnblogs.com/kevingrace/p/5690241.html),.gitignore只是忽视未被加入到远程仓库中的文件

### git合并本地的几个commit操作

### 如果远程仓库里没有东西

1. 进入本地指定目录
2. git init 创建本地仓库
3. 把本地仓库中的文件们加入本地仓库 git add .
4. 关联远程仓库 git remote add origin git@git.oschina.net:yourname/demo.git
5. 更新 git pull

### 导出日志

```
git log --date=iso --pretty=format:'"%h","%an","%ad","%s"' >log.csv
```