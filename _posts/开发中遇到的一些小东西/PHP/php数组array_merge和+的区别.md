---
title: php数组array_merge和+的区别
date: 2019-6-13
categories: PHP
tags: 
- PHP
- 实用代码片段
---

```php
<?php

class compare_array_merge_and_plus
{
    public $a1 = ["1" => "red", "6" => "green"];
    public $a2 = ["a" => "red", "b" => "green"];
    public $b1 = ["3" => "blue", "6" => "yellow"];
    public $b2 = ["c" => "blue", "b" => "yellow"];

    private $result = [];

    public function __destruct()
    {
        print_r($this->result);
    }

    /**
     * array_merge
     * 索引数组直接合并并且索引会重排
     * 关联数组同键名会后者替换前者
     */
    public function merge()
    {
        $this->result['no'] = array_merge($this->a1, $this->b1);
        $this->result['re'] = array_merge($this->a2, $this->b2);
    }

    /**
     * plus
     * 索引数组和关联数组都是同键名前者替换后者，
     */
    public function plus()
    {
        $this->result['no'] = ($this->a1 + $this->b1);
        $this->result['re'] = ($this->a2 + $this->b2);
    }

    public function only_merge()
    {
        $this->result[] = array_merge($this->a1);
        $this->result[] = array_merge($this->a2);
    }

}

(new compare_array_merge_and_plus())->plus();

```

对于同键名的情况

| 同键名 | 索引数组 | 关联数组 |
| ----------- | ----------- | ----------- |
| array_merge | 重新计算索引 | 后者覆盖前者 |
| plus        | 前者覆盖后者 | 前者覆盖后者 |
