---
title: php函数积累
date: 2018-09-06
categories: 
- PHP
- PHP基础知识
tags: PHP基础知识
---

### register_shutdown_function

### array_multisort

> bool array_multisort ( array &$array1 [, mixed $array1_sort_order = SORT_ASC [, mixed $array1_sort_flags = SORT_REGULAR [, mixed $... ]]] )

[array_multisort()](http://php.net/manual/zh/function.array-multisort.php) 可以用来一次对多个数组进行排序，或者根据某一维或多维对多维数组进行排序。

这种类似于从sql语句中的sort by排序还是挺有用的.
```php
<?php
$data = [
    [
        'class' => 'math',
        'score' => 90,
    ],
    [
        'class' => 'english',
        'score' => 70,
    ],
    [
        'class' => 'computer',
        'score' => 70,
    ],
    [
        'class' => 'math',
        'score' => 60,
    ],
    [
        'class' => 'computer',
        'score' => 90,
    ],
    [
        'class' => 'math',
        'score' => 30,
    ],
    [
        'class' => 'computer',
        'score' => 40,
    ],
    [
        'class' => 'english',
        'score' => 30,
    ],
    [
        'class' => 'math',
        'score' => 20,
    ],
    [
        'class' => 'english',
        'score' => 90,
    ],
];

foreach ($data as $key => $value) {
    $column['class'][$key] = $value['class'];
    $column['score'][$key] = $value['score'];
}
// 类似于sql中的 select * from student order by `class` asc, score desc;
array_multisort($column['class'], SORT_ASC, SORT_STRING , $column['score'], SORT_DESC, $data);
var_dump($data);
```
 

