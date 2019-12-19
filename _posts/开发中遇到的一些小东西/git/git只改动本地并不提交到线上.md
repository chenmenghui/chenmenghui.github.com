---
title: git只改动本地并不提交到线上
date: 2019-5-10 13:32:40
categories: git
tags: git
---

clone代码之后,把项目中的.env配置文件修改之后,即使将其加入了.gitignore文件,也被提交了

使用
```
git update-index --assume-unchanged .env
```
才没提交

[原来如此](https://www.cnblogs.com/kevingrace/p/5690241.html),.gitignore只是忽视未被加入到远程仓库中的文件
