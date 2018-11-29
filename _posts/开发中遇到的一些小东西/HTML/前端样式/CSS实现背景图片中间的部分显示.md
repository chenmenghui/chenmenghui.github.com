---
title: CSS实现背景图片中间的部分显示
date: 2018-11-29
categories: 
- css
tags: css
---

具体而言就是透过一个小div来展示一个大图片中间部分的内容

```css
.game-image{
    width: 80px;
    height: 80px;
    background-position: center center ;
    background-repeat: no-repeat;
    background-size: cover;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    border-radius: 5px;
}
```
