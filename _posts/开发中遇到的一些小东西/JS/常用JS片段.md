---
title: 常用js片段
date: 2018-11-29
categories: js
tags: 
- js运用
- 实用代码片段
---

### 单选菜单之类的

```js
var selectCategory = function () {
    $(this).addClass('selected').siblings('.selected').removeClass('selected');
};
```
