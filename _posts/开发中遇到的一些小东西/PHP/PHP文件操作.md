---
title: PHP文件操作
date: 2018/11/07
categories: PHP
tags: 
- PHP
- 实用代码片段 文件
---

### 创建多级文件

其实也就是用到了mkdir这个功能.至于创建文件,用的最多的其实是file_put_contents
```php
if (!is_file(__DIR__.'/test/hello/world/test.txt')) {
    if (!is_dir(__DIR__.'/test/hello/world')) {
        $d = mkdir(__DIR__.'/test/hello/world', 0755, true); // 第三个参数即是否递归地创建目录
    }
    $handle = fopen(__DIR__.'/test/hello/world/test.txt','w'); // 创建文件
    fclose($handle);
}
function createDir($dir) {

}
```
