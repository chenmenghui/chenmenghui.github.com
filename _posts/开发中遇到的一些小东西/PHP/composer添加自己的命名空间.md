---
title: composer添加自己的命名空间
date: 2019-12-31
categories: PHP
tags: 
- PHP
- composer
---

这个其实读文档应该就可以了，只是自己一直以来没有这么做过。好吧，昨儿遇见了，就在此说一下吧。

添加的composer.json中的autoload中，像这样
```json
{
  "name": "valen/simple_test",
  "authors": [
    {
      "name": "Valen",
      "email": "chenmenghui_008@163.com"
    }
  ],
  "autoload": {
    "psr-4": {
      "app\\": "app/",
      "core\\": "core/"
    },
    "files": [
      "common/function/dump.php",
      "common/config/config.php"
    ]
  },
  "require": {
    "ext-mysqli": "*",
    "ext-json": "*"
  }
}

```
其中prs-4就是自己的命名空间，而files指的就是直接引用的文件
然后运行
```
composer dumpautoload
```
该命令将composer.json中的autoload部分更新到相应的命名空间配置文件中
```
├─vendor
│  │  autoload.php
│  │
│  └─composer
│          autoload_files.php
│          autoload_psr4.php
```
比如这两个文件就保存上述json中autoload配置