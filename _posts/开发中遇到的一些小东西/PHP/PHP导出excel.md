---
title: PHP导出excel
date: 2018/10/30
categories: PHP
tags: 
- PHP
- 实用代码片段
---

就是拼接字符串,类似于csv格式.不过需要注意header头

拼接成如下格式的字符串
```
(id)\t(title)\t(content)\t(status)\t\n
(1)\t(title)\t\(content)\t(1)\t\n
```
```php
<?php
header("Content-Type:application/vnd.ms-excel");
header("Content-Disposition:attachment;filename=test.xls");

$head = ['id', 'title', 'content', 'status'];
$body = [
    [1, 'title', 'content', 1],
    [2, 'title', 'content', 2],
    [3, 'title', 'content', 3]
];

array_unshift($body, $head);

$table = array_map(function ($row) {
   return implode("\t", $row) ;
}, $body);
$table = implode("\n", $table);

echo iconv('utf-8', 'gbk//TRANSLIT', $table);
```
