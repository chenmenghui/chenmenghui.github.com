---
title: git如何把一个本地文件夹上传至远程仓库
date: 2018-09-07
categories: git
tags: git
---
### 如果远程仓库里没有东西
1. 进入本地指定目录
2. git init 创建本地仓库
3. 把本地仓库中的文件们加入本地仓库 git add .
4. 关联远程仓库 git remote add origin git@git.oschina.net:yourname/demo.git
5. 更新 git pull

### 如果远程仓库里有东西,同时不需要本地的文件了
1. 在上述4步骤后使用
    1. git fetch -all
    2. git reset --hard
    3. 好吧,其实就是线上线下文件保持一致~~~
    
### 如何合并线上线下文件?
1. 暂时没有遇到,用的再说