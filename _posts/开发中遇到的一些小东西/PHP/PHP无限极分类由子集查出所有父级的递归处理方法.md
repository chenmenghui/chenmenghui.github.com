---
title: PHP无限极分类由子集查出所有父级的递归处理方法
date: 2019-6-14
categories: PHP
tags: 
- PHP
- 实用代码片段
---

核心代码
```php
<?php

$main = function ($child_id) use ($records, &$main) {
    static $list = [];
    $child_list = array_filter($records, function ($value) use ($child_id) {
        return in_array($value['id'], $child_id);
    });
    $parent_id = array_unique(array_column($child_list, 'pid'));
    $list += $child_list;
    if ([0] == $parent_id) {
        return $list;
    } else {
        return $main($parent_id);
    }
};
```

调用方式
```php
<?php

$records = [
    [
        'id'  => 1,
        'pid' => 0,
    ],
    [
        'id'  => 2,
        'pid' => 0,
    ],
    [
        'id'  => 3,
        'pid' => 0,
    ],
    [
        'id'  => 4,
        'pid' => 3,
    ],
    [
        'id'  => 5,
        'pid' => 4,
    ],
    [
        'id'  => 6,
        'pid' => 5,
    ],
    [
        'id'  => 7,
        'pid' => 1,
    ],
    [
        'id'  => 8,
        'pid' => 2,
    ],
    [
        'id'  => 9,
        'pid' => 7,
    ],
    [
        'id'  => 10,
        'pid' => 7,
    ],
];

$list = $main([6, 10]);
```
