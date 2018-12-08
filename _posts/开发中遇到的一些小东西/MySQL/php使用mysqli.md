---
title: php使用mysqli
date: 2018-12-6
categories: mysql
tags: 
- PHP
- mysql
- 实用代码片段
---

mysqli::prepare()可以避免sql注入,还是挺有用的.但是无法同时执行多个sql语句

```php
<?php
/**
* select
 */
$connect = new mysqli('localhost', 'root', '', 'test');
$connect->set_charset('utf8');
$sql = "SELECT `name`, `age` FROM `user` WHERE `name` = ?";
$tmp = $connect->prepare($sql);
$tmp->bind_param('s', $name);
$name = 'grey';
$tmp->bind_result($name, $price);
$tmp->execute();
$tmp->store_result();
$count = $tmp->num_rows(); // 数量
echo $count."\n";
while ($tmp->fetch()) {
    echo "{$name}--{$price}\n";
}
$connect->close();
```
```php
<?php
/**
 * insert
 */
$connect = new mysqli('localhost', 'root', '', 'test');
$connect->set_charset('utf8');
$sql = "INSERT INTO user (`name`, `age`) VALUE (?, ?)";
$tmp = $connect->prepare($sql);
if(!$tmp ) {
    die($connect->error);
}
$tmp->bind_param('sd', $name, $age);

$names = array("Neo", "Morpheus", "Trinity", "Cypher", "Tank");
for ($i = 0; $i < 3; $i++) { // 一次进行多次操作,以execute之前的最后一个（被绑定的）参数为准，即使这个参数在prepare之前
    $name = $names[array_rand($names)];
    $age = $i;
    $tmp->execute();
}

$connect->close();
```