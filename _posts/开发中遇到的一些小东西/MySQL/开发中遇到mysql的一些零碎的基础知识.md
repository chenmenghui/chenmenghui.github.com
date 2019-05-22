---
title: 开发中遇到mysql的一些零碎的基础知识
date: 2018-09-06 19:45:55
categories: mysql
tags: 
- mysql
- 实用代码片段
---

### mysql一行信息中匹配特定的数据
```sql
SELECT * FROM user WHERE nick_name REGEXP '[[:<:]]不觉风止[[:>:]]';
```
如果是类似"1,2,3,4"这种,不用正则也可以
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

### INSERT另外一个表的数据
```sql
INSERT INTO favorite_num (rid, favorite_num)
  SELECT
    rid,
    count(id)
  FROM store_favorite
  GROUP BY rid;
```

两个表的字段顺序对应上就可以了

### mysql5.7 mysql5.7默认开启了ONLY_FULL_GROUP_BY,有时需要将其关闭

SET SESSION sql_mode = 'STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION ';

### 比较两个表中的数据是否一致

```sql
SELECT id,title
FROM (
       SELECT id,title
       FROM text1
       UNION ALL
       SELECT id,title
       FROM text2
     ) tb1
GROUP BY id,title
HAVING count(*) = 1
ORDER BY id, title
```

### 通过另一个表的数据更新当前表

```sql
UPDATE vip_dynamic_reward_record record, vip_member member
SET record.market = member.market
WHERE record.member_child_id = member.id;
```
或者子句的模式
```sql
UPDATE vip_dynamic_reward_record
SET market = (SELECT market FROM vip_member WHERE vip_dynamic_reward_record.member_child_id = vip_member.id);

UPDATE user
SET average_score = (SELECT avg(score) FROM score WHERE user_id = user.id);

UPDATE vip_member
SET dynamic_reward = (SELECT ifnull(sum(times * least(left_dynamic_reward, right_dynamic_reward) * 2 * 0.03 * 0.95), 0)
                      FROM vip_dynamic_reward
                      WHERE vip_member.id = vip_dynamic_reward.member_id);
```
令我奇怪的是,这里的set之后居然没有where限定条件也能正常运行

### 创建一个临时表并把另一个表的数据插入进去

```sql
CREATE TEMPORARY TABLE IF NOT EXISTS record1
SELECT * FROM vip_dynamic_reward_record;
```

原来如此,创建表的时候不一定需要先定义字段再插入数据,可以直接把一个子表的内容插进去的.

### mysql中获取一列数据中几栏里的最小值

```sql
SELECT `member_id`,least(sum(`left_dynamic_reward`), sum(`right_dynamic_reward`))*2*0.03*0.95 `dynamic_reward`
FROM `vip_dynamic_reward`
WHERE `times` < 25
GROUP BY `member_id`
```

### MySQL中的if

```sql
-- IF(expr1,expr2,expr3)
SELECT if (true, 1, 0);
```

```sql
-- IFNULL(expr1,expr2)
SELECT ifnull(null, 1);
```

```sql
SELECT CASE sex
WHEN 1 THEN '男' 
　　ELSE '女' 
END
FROM test;
```

### mysql中的时间

- DATE_FORMAT() 函数用于以不同的格式显示日期/时间数据。
- MySQL 格式化函数 FROM_UNIXTIME()

具体内容参考链接
[mysql格式化日期](https://www.cnblogs.com/dest/p/4205371.html)


### 删除表记录后重新设置自增索引值

```sql
ALTER TABLE `system_menu`
    AUTO_INCREMENT = 60;
```
