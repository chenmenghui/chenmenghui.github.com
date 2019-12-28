---
title: 获取get中的参数
date: 2019-12-23
categories: js
tags: 
- js运用
- 实用代码片段
---

```js
function GetRequest() {
    var url = location.search; //获取url中含"?"符后的字串
    var theRequest = new Object();
    if (url.indexOf("?") != -1) {
        var str = url.substr(1);
        strs = str.split("&");
        for (var i = 0; i < strs.length; i++) {
            theRequest[strs[i].split("=")[0]] = decodeURIComponent(strs[i].split("=")[1]);
        }
    }
    return theRequest;
}
var url = GetRequest();
console.log("url:", url);
```
