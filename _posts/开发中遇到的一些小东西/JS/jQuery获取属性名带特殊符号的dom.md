---
title: jQuery获取属性名带特殊符号的dom
date: 2019-12-23
categories: js
tags: 
- js运用
- 实用代码片段
---

像
```html
<input value="2" name="frm_schedule[2][ScheduleMethod]" type="radio" id="frm_schedule[2][ScheduleMethod]">
```
这个，使用
```js
console.log($('#frm_schedule[2][ScheduleMethod]').attr('id'));
```
是获取不到值的
使用
```js
console.log($('input[id="frm_schedule[2][ScheduleMethod]"]').attr('id'));
```
才行~~~

