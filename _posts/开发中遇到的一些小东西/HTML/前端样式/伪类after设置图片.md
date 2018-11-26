---
title: 伪类after设置图片
date: 2018-11-19
categories: 
- css
tags: css
---

:after中的图片直接使用content引用时,我不知道如何控制图片大小.
所以就使用了背景图的方式实现需求.

![示例图](https://wx2.sinaimg.cn/mw1024/00784LeJly1fxdh5msa5dj30u01qg0xl.jpg)

如图,右方的小勾使用after实现的,部分代码如下

```html
<div class="village-list">
<div class="village-item selected">
    <div class="village-name">${name}</div>
    <div class="village-address">
        <text>地址:</text>
        <text>${address}</text>
    </div>
</div>
</div>
```
```css
.village-list .village-item {
    padding: 20px 0;
    border-bottom: 1px solid #F2F2F2;
    position: relative;
}
.village-list .village-item.selected:after {
    content: '';
    background-image: url(../../image/selected.png);
    background-size: contain;
    background-repeat: no-repeat;
    position: absolute;
    width: 20px;
    height: 20px;
    z-index: 100;
    right: 20px;
    top: calc(50% - 10px);
}
```
