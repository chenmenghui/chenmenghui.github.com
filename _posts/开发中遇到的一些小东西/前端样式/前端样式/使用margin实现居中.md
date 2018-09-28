---
title: 使用margin实现居中
date: 2018-09-18
categories: 
- css
tags: css
---
PC端主要是考虑到可能不兼容flex布局,否则也不会使用这种方式
```css
div{
    background: #FF0000;
    width: 200px;
    height: 200px;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
}
```
### 原理
解释下原理:

1.在普通内容流中，margin:auto的效果等同于margin-top:0;margin-bottom:0。

2.position:absolute使绝对定位块跳出了内容流，内容流中的其余部分渲染时绝对定位部分不进行渲染。

3.为块区域设置top: 0; left: 0; bottom: 0; right: 0;将给浏览器重新分配一个边界框，此时该块块将填充其父元素的所有可用空间，所以margin 垂直方向上有了可分配的空间。

4.再设置margin 垂直方向上下为auto，即可实现垂直居中。（注意高度得设置）。
[资料来源](https://blog.csdn.net/linshizhan/article/details/71521140)