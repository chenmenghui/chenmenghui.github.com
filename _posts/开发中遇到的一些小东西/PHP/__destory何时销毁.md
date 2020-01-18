---
title: __destroy何时销毁
date: 2020-1-18
categories: PHP
tags: 
- PHP
---

开发中遇见一个文件，
```php
<?php
class Db
{
    /**
     * @return mysqli
     */
    public static function init()
    {
        if (!self::$database) {
            $database = new mysqli(
                self::$db_config['host'],
                self::$db_config['user'],
                self::$db_config['password'],
                self::$db_config['dbname']
            );
            $database->set_charset('utf8');
            self::$database = $database;
        }
        return self::$database;
    }

    // ...

    public function __destruct()
    {
        self::init()->close();
    }
}
```
在这样调用的时候
```php
$res = (new Db())->table('user')
    ->set([
        'name'     => 'grey',
        'password' => md5('1111'),
    ])
    ->save();
var_dump(Db::init()->insert_id);
```
发现这个mysqli链接已经被关闭了。
删除Db类中__destroy，就可以使用。

但是，如果是这样写的话
```php
$db = new Db();
$res = $db->table('user')
    ->set([
        'name'     => 'grey',
        'password' => md5('1111'),
    ])
    ->save();
var_dump(Db::init()->insert_id);
```
是有效的。

所以，这个__destroy会在什么时候被触发呢？