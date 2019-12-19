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
echo preg_filter("/(,.*$)|$/", '', $item['image']);
```

以后会找到更好的方法吧.