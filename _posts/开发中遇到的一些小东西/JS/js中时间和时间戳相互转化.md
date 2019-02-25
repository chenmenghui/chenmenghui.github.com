---
title: 注意箭头函数
date: 2018-11-26
categories: js
tags: 
- js运用
- 实用代码片段
---

箭头函数完全修复了this的指向，this总是指向词法作用域，也就是外层调用者obj：

查看下面一个例子
```js
$('.sort-area').on('click', '.sort-item', () => {
    console.log("$(this).attr('class')", $(this).attr('class'));
});
```
其中的this指向的是window

而匿名函数的写法里
```js
$('.sort-area').on('click', '.sort-item', function() {
    console.log("$(this).attr('class')", $(this).attr('class'));
});
```
this指向的是事件的触发者
