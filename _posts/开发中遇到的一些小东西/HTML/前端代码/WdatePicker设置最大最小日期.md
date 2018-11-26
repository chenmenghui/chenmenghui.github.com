---
title: WdatePicker设置最大最小日期
date: 2018-09-06 19:50:59
categories: js
tags: 
- js运用
- 实用代码片段
---
# WdatePicker设置最大最小日期
```js
onclick="WdatePicker({maxDate:'#F{$dp.$D(\'endTime\')||\'new Date()\'}'})"
onclick="WdatePicker({minDate:'#F{$dp.$D(\'startTime\')}',maxDate:new Date()})"
```
