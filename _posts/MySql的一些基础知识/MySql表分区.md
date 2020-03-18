---
title: MySql表分区
date: 2018-12-20
categories: 
- mysql
tags: MySQL
---

### 什么是表分区

分区是将一个大表,根据条件分割成若干个小表.mysql5.1开始支持表分区,5.5支持表分区的所有函数

### 为什么要对表进行分区

> 为了改善大型表以及具有各种访问模式的表的可伸缩性,客观理性和提高数据库效率

分区的一些优点包括
- 与单个磁盘或文件系统相比,可以存储更多的数据
- 通过删除与增加那些数据有关的分区,很容易地删除或增加那些数据(比如直接truncate一张表比delete where快多了)
- 一些查询可以得到极大的优化
- 通过跨多个磁盘甚至服务器来分散数据查询,来获得更高的查询吞吐量
- MySql5.5之后支持所有函数的分区优化

### 基本分区类型

#### RANGE分区

- 基于属于一个给定连续区间的列值,把多行分配给分区
- 这些区间要连续且不能相互重叠,使用values less than操作符来进行定义
```sql
ALERT TABLE titles
partition by range(year(from_date))
(
  partion p01 values less than (1985),
  partion p02 values less than (1995),
  partion p03 values less than (2005),
  partion p04 values less than (2015),
  partion p05 values less than (MAXVALUE),
);
```
```sql
CREATE TABLE t1
(
  id   int,
  name varchar(20),
  age  int
)
  PARTITION BY RANGE (age)
    (
    PARTITION p01 VALUES LESS THAN (10),
    PARTITION p02 VALUES LESS THAN (20),
    PARTITION p03 VALUES LESS THAN (30),
    PARTITION p04 VALUES LESS THAN (MAXVALUE)
    );
```

#### LIST分区

- 类似于按RANGE分区,区别在于LIST分区是基于列值匹配一个离散值集合中的某个值来进行选择
- LIST分区通过使用"partition by list(expr)"来实现,其中"expr"是某列值或一个基于某个列值并返回一个证书的表达式,然后通过"value in (value_list)"的方式来定义每个分区,其中"value_list"是一个通过通过逗号分隔的整数列表
- 在MySQL5.1中,list分区默认只能匹配整数表.现在5.5已经解决
```sql
CREATE TABLE t2
(
  id       int,
  cid      int,
  NAME     VARCHAR(20),
  pos_date datetime
)
  PARTITION BY LIST (cid)
    (
    PARTITION p01 VALUES IN (1,2,3),
    PARTITION p02 VALUES IN (4,5,6),
    PARTITION p03 VALUES IN (7,8,9)
    );
```

#### HASH分区

- 基于用户定义的表达式的返回值来进行选择的分区,该表达式使用将要插入到表中的这些行的列值进行计算.这个表达式可以包含MySQL中有效的,产生非负整数值的任何表达式
- 使用HASH分区的优点在于数据分布较为均匀

```sql
CREATE TABLE t3
(
  id       int,
  cid      int,
  name     varchar(20),
  pos_date datetime
)
  PARTITION BY HASH (cid)
    PARTITIONS 4
```

#### linear hash分区

- 线性与常规哈希的区别在于,线性哈希功能使用的一个线性的2的幂(powers-of-two)运算规则,而常规哈希使用的时求哈希函数值的模数.
- 按照线性哈希分区的优点在于增加 删除 合并 和 拆分分区将变得更加快捷,有利于处理含有极大量(1t)数据的表.不过,mysql的线性哈希算法导致相比较常规哈希,数据可能分布的不那么均匀,容易产生"hotspot nodes"
```sql
CREATE TABLE t3
(
  id       int,
  cid      int,
  name     varchar(20),
  pos_date datetime
)
  PARTITION BY linear HASH (cid)
    PARTITIONS 4
```

### KEY分区

- 类似于哈希分区,哈希分区使用的用户定义的表达式,而key分区的哈希函数是由mysql服务器提供.mysql簇(cluster)使用函数md5()来实现key分区;对于使用其他存储引擎的表,服务器使用其自己内部的哈希函数.这些函数时基于password()一样的运算法则

```sql
CREATE TABLE t4
(
  id       int,
  cid      int,
  name     varchar(20),
  pos_date datetime
)
  PARTITION BY [linear] KEY (cid)
    PARTITIONS 4;
```

### 多列分区

- columns关键字允许字符串和日期列作为分区定义列,同时还允许使用多个列定义一个分区

```sql
CREATE TABLE t5
(
  id       int,
  cid      int
)
  PARTITION BY RANGE COLUMNS (id, cid)
    (
    PARTITION p01 VALUES LESS THAN (10,10),
    PARTITION p02 VALUES LESS THAN (10,20),
    PARTITION p03 VALUES LESS THAN (10,30),
    PARTITION p04 VALUES LESS THAN (10,MAXVALUE ),
    PARTITION p05 VALUES LESS THAN (MAXVALUE ,MAXVALUE )
    );
```

### 子分区

- 子分区是分区别中每个分区的再次分割
- 子分区可以用于特别大的表,在多个磁盘间分配数据和索引
