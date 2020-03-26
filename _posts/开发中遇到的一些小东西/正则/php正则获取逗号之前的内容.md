---
title: php正则获取逗号之前的内容
date: 2019-6-11
categories: 正则
tags: 
- 正则
- 实用代码片段
---

原始字符串如"1231312"或"123,123"

```php
echo preg_filter("/(,.*$)|$/", '', $str);
```

好吧，就是把匹配到的字符替换成空了。总觉的有些怪怪的。

已经找到另一种方法，就是把preg_match那个稍稍封装一下就行了。

```php
    function pregGetter($pattern, $subject, $part = 'value')
    {
        preg_match($pattern, $subject, $match);
        return $match[$part];
    }
```