---
title: PHP批量重命名文件
date: 2019-12-21
categories: PHP
tags: 
- PHP
- 实用代码片段
---

看《SQL进阶》，只是随书附带代码后缀居然是txt，看着不爽，就想批量重命名成sql。
没有什么难点，但是自己长时间不写类似的代码居然不能第一时间搞定。什么时候自己变得那么菜了。
好吧，一点一点dump试出来的，实在是难受。

```php
function deep($dir)
{
    if (is_dir($dir)) {
        $current_files = scandir($dir);
        foreach ($current_files as $item) {
            if ($item != '.' && $item != '..' && $item != './idea') {
                $current_file = $dir . '/' . $item;
                if (is_dir($current_file)) {
                    deep($current_file);
                } else {
                    if ($new_name = preg_replace('/txt$/', 'sql', $current_file)) {
                        rename($current_file, $new_name);
                    }
                }
            }
        }
    }
}

deep('./');
```
