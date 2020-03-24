---
title: jQuery一些不常见的属性选择器
date: 2019-12-23
categories: js
tags: 
- js运用
- 实用代码片段
---

### jQuery获取属性名带特殊符号的dom

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

### 使用正则

```js
let _select = $(this).find('select[id^="frm_schedule"][id$="\[Point\]"]');
```

注意其中 id 的值「^=」和「$=」同时匹配 id 的头和尾。即「且」的关系。

如果写成

```js
let _select = $(this).find('select[id^="frm_schedule"],[id$="\[Point\]"]');
```

这就是「或」的关系