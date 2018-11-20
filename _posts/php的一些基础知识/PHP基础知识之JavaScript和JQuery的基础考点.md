---
title: PHP基础知识之JavaScript和JQuery的基础考点
date: 2018-09-06
categories: 
- PHP
- PHP基础知识
tags: PHP基础知识
---
JavaScript基础语法

变量的定义
变量名可以是字符_$
变量名称对大小写敏感
使用var关键字声明变量
未使用值声明的变量,值是undefined
如果重新声明JavaScript变量,该变量的值不会丢失

数据类型

JavaScript变量均为对象.声明一个变量,就创建了一个新的对象

创建对象
- new Object()
- 使用对象构造器
- json对象

函数
不支持默认值
函数内部声明的变量(使用var)是局部变量
在函数外声明的变量全局可访

else if必须分开写

JavaScript内置对象

- Number
    - var pi = 3.14
    - var myNum = new Number(value)
    - var myNum = Number(value)
    
- String
    - var str = 'Hello' // 单引号双引号无区别
    - var str = new String(s)
    - var str = String(s)
    
- Boolean
    - var bol = true
    - var bol =  New Boolean(value)
    - var bol = Boolean(value)
    
- Array
    - var arr = new Array();
    - var arr = new Array(size);
    - var arr = new Array(e1,e2,...)
    
- Date
    - var date = new Date();
    
- Math
    - var pi = Math.PI;

- RegExp
    - /pattern/attributes
    - new RegExp(pattern, attributes)
    
延伸考点:window对象
- window
- Navigator
- Screen
- History
- Location

延伸考点:DOM对象
- Document
- Element
- Attr
- Event

延伸考点:jQuery基础知识
- jQuery选择器
- jQuery事件
- jQuery效果
- jQuery DOM操作
    - 属性
    - 值
    - 节点
    - css
    - 尺寸
    
