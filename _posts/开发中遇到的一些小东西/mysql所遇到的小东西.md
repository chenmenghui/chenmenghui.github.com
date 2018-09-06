---
layout: mysql中on
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

### mysql中join之后的条件
看一个sql语句
```sql
SELECT
  n.cat_id,
  c.cat_id
FROM site_news n LEFT JOIN site_news_category c
    ON n.cat_id = c.cat_id AND c.cat_id = 21;
```
它的结果居然是这个样子的:
![sql result](/img/20180906-sql.png)
命名都把site_news_category.cat_id设置成21了...甚至on之后在加一个条件 and c.cat_id is not null,其结果集也是如此

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

### mysql把搜索到的内容展示成一行
```sql
SELECT group_concat(key) FROM config WHERE group = 'points';
```

