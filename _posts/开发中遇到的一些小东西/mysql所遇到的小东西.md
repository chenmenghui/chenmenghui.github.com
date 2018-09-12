---
layout: 开发中遇到mysql的一些零碎的基础知识
title: mysql中的正则定界符
date: 2018-09-06 19:45:55
categories: mysql
tags: mysql零碎的基础知识
---

### mysql一行信息中匹配特定的数据
```sql
SELECT * FROM user WHERE nick_name REGEXP '[[:<:]]不觉风止[[:>:]]';
```
不用正则也可以
```sql
SELECT * FROM user WHERE find_in_set(nick_name, '不觉风止');
```
用like就太繁琐了,需要考虑多种情况

### mysql中join之后的on
看一个sql语句
```sql
SELECT
  n.cat_id,
  c.cat_id
FROM site_news n LEFT JOIN site_news_category c
    ON n.cat_id = 1
```
他的结果集居然是site_news的所有的记录。

把sql语句改成这个样子
```sql
SELECT
  n.cat_id,
  c.cat_id
FROM site_news n LEFT JOIN site_news_category c
    ON n.cat_id = c.cat_id
WHERE c.cat_id = 21;
```
其结果就符合预期了.

[百度之后,看到这样的结论](https://blog.csdn.net/qq_33864656/article/details/77838258)

> 假如是左连接的话，如果左边表的某条记录不符合连接条件，那么它不进行连接，但是仍然留在结果集中（此时右边部分的连接结果为NULL）

才知道困惑我的原因竟在于此

原来,on只是作为两个表的链接条件而使用的.具体的筛选,还需要使用where处理

### mysql中的on、where、having之间的区别 
    
在select语句中，这三者筛选是有时序的。on最先，where次之，having最后。

在通过两张及两张以上的表链接来返回记录时，首先会产生一个中间的临时表。on是产生临时表所需要的条件。同时，记住上述所说的话。

之后where是在这张临时表的基础之上起作用的。另外where的条件是数据表中原有的字段。

然后就是having在where之后的基础起作用。having对聚合函数及as产生的别名有效

### mysql把搜索到的内容展示成一行
```sql
SELECT group_concat(key) FROM config WHERE group = 'points';
```

### UPDATE把一个表中的字段与另一个表的字段对应上
```sql
update store_reply s, base_user c
set s.head_img_url = c.head_img_url
where s.user_id = c.id;
```

