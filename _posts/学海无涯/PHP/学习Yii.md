---
title: 学习Yii
date: 2019-2-11
categories: 
- PHP
tags: 
- PHP
---

### 依赖注入 控制反转 依赖倒置

[依赖倒置、控制反转和依赖注入辨析](https://blog.csdn.net/moreorless/article/details/4510859)

控制反转，英文 Inversion of Control，简称 IOC，字面上可以理解为对xx的控制进行了一个反转，换句话说就是对xx的控制的另一种实现，是一种思路，一种逻辑思想。这个很容易理解。

依赖注入就是把对象A所依赖的其他对象B呀C呀的，以属性或者构造函数的方式传递到对象A的内部，而不是直接在对象A内实例化。
其目的就是为了让对象A和其依赖的其他对象解耦，减少二者的依赖。即通过“注入”的方式去解决依赖问题

依赖倒置原则（Dependence Inversion Principle, DIP），是一种软件设计思想。传统软件设计中，上层代码依赖于下层代码，当下层出现变动时， 上层代码也要相应变化，维护成本较高。而DIP的核心思想是上层定义接口，下层实现这个接口， 从而使得下层依赖于上层，降低耦合度，提高整个系统的弹性。这是一种经实践证明的有效策略。

撇开依赖注入,实现的方式可以是这样
```php
<?php
class EmailSender
{
    public function send()
    {
        echo '发送邮件';
    }
}

class User
{
    public function register()
    {
        $email = new EmailSender;
        $email->send();
    }
}
```
如下修改之后
```php
<?php
/**
 * Created by PhpStorm.
 * User: Administrator
 * Date: 2019/2/11
 * Time: 16:18
 */

interface EmailSender{
    public function send();
}

class EmailSenderByQq implements EmailSender
{
    public function send()
    {
        echo '通过qq发送邮件';
    }
}

class User
{
    private $_emailSenderObject;
    public function __construct(EmailSender $_emailSenderObject)
    {
        $this->_emailSenderObject = $_emailSenderObject;
    }
    public function register()
    {
        $this->_emailSenderObject->send();
    }
}

$user = new User(new EmailSenderByQq()); // 依赖注入 依赖倒置原则
$user->register();
```
遇到替换发送邮件类时,就不需要在User类内部进行替换了(即解耦);

所谓注入,就是把一个实例传递到另一个实例内部.


