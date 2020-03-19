---
title: plantUML生成的图片过大时无法显示全部
date: 2020-3-9
categories: 
- 工具
tags: plantUML
---

实际操作的时候发现，这个plantUML转化的图片大小是有个默认最大值(4086)。普通情况下超出部分不予显示，但缩放的话，字体就看不清了。

因为使用的是phpstorm，找了半天居然没有找到解决办法。也是在误打误撞的情况下，发现在命令行直接输入
```cmd
java -DPLANTUML_LIMIT_SIZE=8192 -jar /path/to/plantuml.jar ...
```
可以生成比较大的图。

既然如此，就在「phpstorm64.exe.vmoptions」把「-DPLANTUML_LIMIT_SIZE=8192」添加到最后一行。发现有效。
