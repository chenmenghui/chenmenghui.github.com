---
title: composer添加自己的命名空间
date: 2019-12-31
categories: PHP
tags: 
- PHP
- composer
---

这个其实读文档应该就可以了，只是自己一直以来没有这么做过。好吧，昨儿遇见了，就在此说一下吧。

添加的composer.json中的auto中，然后运行
```
composer dumpautoload
```
该命令将composer.json中的autoload部分更新到相应的命名空间配置文件。
