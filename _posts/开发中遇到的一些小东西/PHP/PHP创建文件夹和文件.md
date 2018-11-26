---
title: PHP创建文件夹和文件
date: 2018/11/07
categories: PHP
tags: 
- PHP
- 实用代码片段
---

其实也就是用到了mkdir这个功能.至于创建文件,用的最多的其实是file_put_contents

```php
<?php
$targetPath = './data/';
if(!file_exists($targetPath)) {
    $dirs = explode('/' , $targetPath);
    $count = count($dirs);
    $path = '.';
    for ($i = 0; $i < $count; ++$i) {
        $path .= '/' . $dirs[$i];
        if(!is_dir($path)){//不是目录
            if(!mkdir($path,0755)){ // 其实mkdir可以递归建立文件夹,如下
                //todo
            }
        }
    }
}
```
```php
<?php
if (!is_file(__DIR__.'/test/hello/world/test.txt')) {
    if (!is_dir(__DIR__.'/test/hello/world')) {
        $d = mkdir(__DIR__.'/test/hello/world', 0755, true); // 第三个参数即是否递归地创建目录
    }
    $handle = fopen(__DIR__.'/test/hello/world/test.txt','w'); // 创建文件
    fclose($handle);
}
```