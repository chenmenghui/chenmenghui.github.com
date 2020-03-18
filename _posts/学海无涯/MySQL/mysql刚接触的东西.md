---
title: MySQL刚接触的东西
date: 2019-12-19
categories: 
- mysql
tags: 
- mysql
---

### USING

using等价于join操作中的on，例如a和b根据id字段关联，那么以下等价
using(id)
on a.id=b.id
以下2个实例等价：
```sql
select a.name,b.age from test as a
join test2 as b
on a.id=b.id
```
```sql
select a.name,b.age from test as a
join test2 as b
using(id)
```

### COALESCE

返回参数中的第一个非空表达式（从左向右依次类推）
```
COALESCE(x, y) = (CASE WHEN x IS NOT NULL THEN x ELSE y END)
```
```sql
SELECT COALESCE(NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, 1); 
-- Return 1 
```

### NATURAL

Natural join即自然连接，natural join等同于inner join或inner using，其作用是将两个表中具有相同名称的列进行匹配
关联的表具有一对或多对同名的列
连接时候不需要使用on或者using关键字

### WITH ROLLUP

在group分组字段的基础上再进行汇总统计数据