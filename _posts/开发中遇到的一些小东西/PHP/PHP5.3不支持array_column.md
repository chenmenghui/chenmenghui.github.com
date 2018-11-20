---
title: PHP5.3不支持array_column
date: 2018/11/20
categories: PHP
tags: 
- PHP
- 实用代码片段
---

公司服务器php是老版的,不支持array_column.但这个函数功能还是挺实用的,自己在处理些订单数组时经常用到.下面的代码是针对二维数组的.

```php
<?php
if (!function_exists('array_column')) {
    function array_column($ori_array, $column, $index_key = null)
    {
        $new_array = [];
        if ($index_key) {
            array_walk($ori_array, function ($value) use (&$new_array, $column, $index_key) {
                $new_array[$value[$index_key]] = $value[$column];
            });
        } else {
            array_walk($ori_array, function ($value) use (&$new_array, $column) {
                $new_array[] = $value[$column];
            });
        }
        return $new_array;
    }
}
```
