---
title: 注意箭头函数
date: 2018-11-26
categories: js
tags: 
- js运用
- 实用代码片段
---

```js

var time2stamp = function(time) {
    time = time.substring(0,19);
    time = time.replace(/-/g,'/');
    return Math.round(new Date(time).getTime()/1000);
};

var stamp2time = function(stamp) {
    var d = new Date(stamp * 1000);    //根据时间戳生成的时间对象
    var date = (d.getFullYear()) + "-" +
        (d.getMonth() + 1) + "-" +
        (d.getDate()) + " " +
        (d.getHours()) + ":" +
        (d.getMinutes()) + ":" +
        (d.getSeconds());
    return date;
};
```
