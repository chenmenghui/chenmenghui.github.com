---
title: SQL进阶
date: 2020-2-4
categories: 
- 图书
---

# 第一章 神奇的SQL

## CASE表达式

### CASE表达式概述

写法
```
-- 简单CASE 表达式
CASE sex
WHEN '1' THEN '男'
WHEN '2' THEN '女'
ELSE '其他' END

-- 搜索CASE 表达式
CASE WHEN sex = '1' THEN '男'
WHEN sex = '2' THEN '女'
ELSE '其他' END
```

注意事项：
- 统一各分支返回的数据类型
- 不要忘记写END
    - 这是个语法结构，是必选项而非可选项。
- 养成写ELSE子句的习惯
    - 不写的话，其不在WHEN条件中的项结果是NULL

### 将已有编号方式转换为新的方式并统计

在进行非定制化统计时，我们经常会遇到将已有编号方式转换为另外一种便于分析的方式并进行统计的需求。

如：
```sql
-- 把县编号转换成地区编号(1)
SELECT CASE pref_name
           WHEN '德岛' THEN '四国'
           WHEN '香川' THEN '四国'
           WHEN '爱媛' THEN '四国'
           WHEN '高知' THEN '四国'
           WHEN '福冈' THEN '九州'
           WHEN '佐贺' THEN '九州'
           WHEN '长崎' THEN '九州'
           ELSE '其他' END AS district,
       SUM(population)
FROM PopTbl
GROUP BY CASE pref_name
             WHEN '德岛' THEN '四国'
             WHEN '香川' THEN '四国'
             WHEN '爱媛' THEN '四国'
             WHEN '高知' THEN '四国'
             WHEN '福冈' THEN '九州'
             WHEN '佐贺' THEN '九州'
             WHEN '长崎' THEN '九州'
             ELSE '其他' END;
```
在mysql等数据库中，可以简化成
```sql
SELECT CASE pref_name
           WHEN '德岛' THEN '四国'
           WHEN '香川' THEN '四国'
           WHEN '爱媛' THEN '四国'
           WHEN '高知' THEN '四国'
           WHEN '福冈' THEN '九州'
           WHEN '佐贺' THEN '九州'
           WHEN '长崎' THEN '九州'
           ELSE '其他' END AS district,
       SUM(population)
FROM PopTbl
GROUP BY district;
```

没错，这里的 GROUP BY 子句使用的正是 SELECT 子句里定义的列的别称——district 。但是严格来说，这种写法是违反标准 SQL 的规则的。**因为 GROUP BY 子句比 SELECT 语句先执行**，所以在 GROUP BY 子句中引用在 SELECT 子句里定义的别称是不被允许的。事实上，在 Oracle、DB2、SQL Server 等数据库里采用这种写法时就会出错。
不过也有支持这种 SQL 语句的数据库，例如在 PostgreSQL 和MySQL 中，这个查询语句就可以顺利执行。这是因为，这些数据库在执行查询语句时，会先对 SELECT 子句里的列表进行扫描，并对列进行计算。不过因为这是违反标准的写法，所以这里不强烈推荐大家使用。但是，这样写出来的 SQL 语句确实非常简洁，而且可读性也很好。

### 用一条SQL语句进行不同条件的统计

进行不同条件的统计是 CASE 表达式的著名用法之一。
例如，我们需要往存储各县人口数量的表 PopTbl 里添加上「性别」列，然后求按性别、县名汇总的人数。具体来说，就是统计表 PopTbl2 中的数据，然后求出如表「统计结果」所示的结果。
```sql
SELECT pref_name, sum(if(sex = 1, population, 0)) AS man, sum(if(sex = 2, population, 0)) as woman
FROM PopTbl2
GROUP BY pref_name;
```
*新手用 WHERE 子句进行条件分支，高手用 SELECT 子句进行条件分支。*

### 用 CHECK 约束定义多个列的条件关系

CASE 表达式和 CHECK 约束是很般配的一对组合

### 在 UPDATE 语句里进行条件分支

```sql
UPDATE Salaries
SET salary = CASE
                 WHEN salary >= 300000 THEN salary * 0.9
                 WHEN salary >= 250000 AND salary <= 280000 THEN salary * 1.2
                 ELSE salary END;
```

### 表之间的数据匹配

在 CASE 表达式里，我们可以使用 BETWEEN 、LIKE和 < 、> 等便利的谓词组合，以及能嵌套子查询的 IN 和 EXISTS 谓词。因此，CASE 表达式具有非常强大的表达能力。
```sql
SELECT course_name,
       IF(course_id IN (SELECT course_id
                        FROM OpenCourses
                        WHERE month = 200706), 'o',
          'x') AS 'June',
       IF(course_id IN (SELECT course_id
                        FROM OpenCourses
                        WHERE month = 200707), 'o',
          'x') AS 'July',
       IF(course_id IN (SELECT course_id
                        FROM OpenCourses
                        WHERE month = 200708), 'o',
          'x') AS 'August'
FROM CourseMaster;
```
这样的查询没有进行聚合，因此也不需要排序，月份增加的时候仅修改 SELECT 子句就可以了，扩展性比较好。
无论使用 IN 还是 EXISTS ，得到的结果是一样的，但从性能方面来说， EXISTS 更好。通过 EXISTS 进行的子查询能够用到“month,course_id ”这样的主键索引，因此尤其是当表 OpenCourses 里数据比较多的时候更有优势。

### 在 CASE 表达式中使用聚合函数

```sql
SELECT std_id,
       CASE
           WHEN count(club_id) = 1 THEN max(club_id)
           ELSE max(CASE WHEN main_club_flg = 'Y' THEN club_id ELSE NULL END)
           END AS main_club_id
FROM StudentClub
GROUP BY std_id;
```
*注意其中的max的用法，这里仅仅是作为一个聚合函数使用的，实际上min、sum等在这里的效果是一样的。不用的话就犯了「SELECT list is not in GROUP BY clause and contains nonaggregated column」的错*

#### 总结

作为表达式，CASE表达式在执行时会被判定为一个固定值，因此它可以写在聚合函数内部；也正因为它是表达式，所以还可以写在SELECE子句、GROUPBY子句、WHERE子句、ORDERBY子句里。简单点说，在能写列名和常量的地方，通常都可以写CASE表达式。

本节要点：
1. 在 GROUP BY 子句里使用 CASE 表达式，可以灵活地选择作为聚合的单位的编号或等级。这一点在进行非定制化统计时能发挥巨大的威力。
2. 在聚合函数中使用 CASE 表达式，可以轻松地将行结构的数据转换成列结构的数据。
3. 相反，聚合函数也可以嵌套进 CASE 表达式里使用。
4. 相比依赖于具体数据库的函数，CASE 表达式有更强大的表达能力和更好的可移植性。
5. 正因为 CASE 表达式是一种表达式而不是语句，才有了这诸多优点。

## 自连接的用法

### 可重排列、排列、组合

```sql
-- 用于获取可重排列的SQL 语句（笛卡尔积）
SELECT P1.name AS name_1, P2.name AS name_2
FROM Products P1, Products P2;

-- 用于获取排列的SQL 语句
SELECT P1.name AS name_1, P2.name AS name_2
FROM Products P1, Products P2
WHERE P1.name <> P2.name;

-- 用于获取组合的SQL 语句
SELECT P1.name AS name_1, P2.name AS name_2
FROM Products P1, Products P2
WHERE P1.name > P2.name;
```

## 删除重复行

```sql
DELETE
FROM Products
WHERE id IN (SELECT id
             FROM ((SELECT p2.id
                    FROM Products p1,
                         Products p2
                    WHERE p1.name = p2.name
                      AND p1.price = p2.price
                      AND p1.id > p2.id)) tmp);
```

## 查询局部不一致的数据

```sql
SELECT DISTINCT p1.name, p1.price
FROM Products p1,
     Products p2
WHERE p1.name != p2.name
  AND p1.price = p2.price;
```

### 排序

```sql
SELECT p1.*, (SELECT count(DISTINCT p2.price) FROM Products p2 WHERE p2.price > p1.price) + 1 AS rank_1
FROM Products p1
ORDER BY rank_1;
```
这段代码的排序方法看起来很普通，但很容易扩展。例如去掉标量子查询后边的 +1 ，就可以从 0 开始给商品排 序，而且如果修改成COUNT(DISTINCT P2.price) ，那么存在相同位次的记录时，就可以不跳过之后的位次，而是连续输出（相当于 DENSE_RANK 函数）。由此可知，这条 SQL 语句可以根据不同的需求灵活地进行扩展，实现不同的排序方式。

```sql
SELECT p1.name, max(p1.price), count(p2.name) + 1 AS rank_1
FROM Products p1
     LEFT JOIN Products p2 ON p1.price < p2.price
GROUP BY p1.name
ORDER BY rank_1;
```
注意这里使用的外连接，如果使用内连接的话会漏掉排在第一位的数据
 
### 本章小结

要点
1. 自连接经常和非等值连接结合起来使用。
2. 自连接和 GROUP BY 结合使用可以生成递归集合。
3. 将自连接看作不同表之间的连接更容易理解。
4. 应把表看作行的集合，用面向集合的方法来思考。
5. 自连接的性能开销更大，应尽量给用于连接的列建立索引。

## 三值逻辑和NULL

一时间难以理解。不过也基本上接触不到，工作的时候基本上都加了not null约束

三值逻辑真值表（NOT)
| x | NOT x |
| --- | --- |
| t | f |
| u | u |
| f | t |

三值逻辑真值表（AND）
| AND | t | u | f |
| --- | --- | --- | --- |
| t | t | u | f |
| u | u | u | f |
| f | f | f | f |

三值逻辑真值表（OR）
| OR | t | u | f |
| --- | --- | --- | --- |
| t | t | t | t |
| u | t | u | u |
| f | t | u | f |

比较简单的记法：
AND的情况： false > unknown > true
OR的情况：true > unknown > false


null是不能应用于比较运算符的。强行使用的话运算结果是「unknown」（未知）。这种结果就导致了运算结果与实际需求相悖。
例如：
```sql
SELECT *
FROM SeqTbl
WHERE seq NOT IN (1, 2, null);
```
是查不到结果的。
解释如下，上个sql可以转化成
```
SELECT *
FROM SeqTbl WHERE seq <> 1 AND seq <> 2 AND seq <> NULL;
```
而「seq <> null」的结果是unknown。
where只有条件为true的情况才会返回值。而「seq <> 1 AND seq <> 2 AND seq <> NULL」的结果是unknown，所以就没有返回值。


### 本届小结

1. NULL 不是值。
2. 因为 NULL 不是值，所以不能对其使用谓词。
3. 对 NULL 使用谓词后的结果是 unknown 。
4. unknown 参与到逻辑运算时，SQL 的运行会和预想的不一样。
5. 按步骤追踪 SQL 的执行过程能有效应对 4 中的情况。

最后说明一下，要想解决 NULL 带来的各种问题，最佳方法应该是往表里添加 NOT NULL 约束来尽力排除 NULL 。这样就可以回到美妙的二值逻辑世界（虽然并不能完全回到）。

## HAVING子句的力量

### 寻找缺失的编号

```sql
-- 这个是有缺陷的，缺1查不到，还会多查一个
SELECT (s1.seq + 1)
FROM SeqTbl s1
WHERE (s1.seq + 1) NOT IN (SELECT s2.seq FROM SeqTbl s2);
```

### 求众数

```sql
SELECT income, COUNT(*) AS cnt
FROM Graduates
GROUP BY income
HAVING COUNT(*) >= ALL (SELECT COUNT(*)
                        FROM Graduates
                        GROUP BY income);
``` 
另外，all/any谓词用在null或空集时会出现问题，可以使用极值函数替代，如
```sql
SELECT count(*) AS num, income
FROM Graduates
GROUP BY income
HAVING num >= (SELECT max(num) FROM (SELECT count(*) AS num, income FROM Graduates GROUP BY income) tmp);
```

### 求中位数

```sql
SELECT t1.value
FROM test t1
JOIN test t2
GROUP BY t1.value
HAVING sum(IF(t1.value >= t2.value, 1, 0)) > COUNT(*) / 2
   AND sum(IF(t1.value <= t2.value, 1, 0)) > COUNT(*) / 2;
```

主要是 「group by」和 「having」的联合使用。这是一种「集合」的求法。使用「排序」的方式会更直观些。

### 查询不包含NULL的集合

COUNT 函数的使用方法有 COUNT(*) 和 COUNT( 列名 ) 两种，它们的区别有两个：第一个是性能上的区别；第二个是 COUNT(*) 可以用于 NULL ，而 COUNT( 列名 ) 与其他聚合函数一样，要先排除掉NULL 的行再进行统计。第二个区别也可以这么理解：COUNT(*) 查询的是所有行的数目，而 COUNT( 列名 ) 查询的则不一定是。

### 用关系除法运算进行购物篮分析

```sql
SELECT SI.shop
FROM ShopItems SI,
     Items I
WHERE SI.item = I.item
GROUP BY SI.shop
HAVING COUNT(SI.item) = (SELECT COUNT(item) FROM Items);
```

### 本章总结

1. 表不是文件，记录也没有顺序，所以 SQL 不进行排序。
2. SQL 不是面向过程语言，没有循环、条件分支、赋值操作。
3. SQL 通过不断生成子集来求得目标集合。SQL 不像面向过程语言那样通过画流程图来思考问题，而是通过画集合的关系图来思考。
4. GROUP BY 子句可以用来生成子集。
5. WHERE 子句用来调查集合元素的性质，而 HAVING 子句用来调查集合本身的性质。

## 外连接的用法

### 用外连接进行行列转换 (1)（行→列）：制作交叉表

```sql
-- 外连接
SELECT C0.name, C1.name, C2.name, C3.name
FROM (SELECT DISTINCT name FROM Courses) C0
LEFT JOIN (SELECT name FROM Courses WHERE course = 'SQL入门') C1 ON C0.name = C1.name
LEFT JOIN (SELECT name FROM Courses WHERE course = 'UNIX基础') C2 ON C0.name = C2.name
LEFT JOIN (SELECT name FROM Courses WHERE course = 'Java中级') C3 ON C0.name = C3.name;

-- 标量子查询
SELECT name,
       if((SELECT count(*) FROM Courses C1 WHERE C0.name = C1.name AND C1.course = 'SQL入门'), 'o', 'x'),
       if((SELECT count(*) FROM Courses C1 WHERE C0.name = C1.name AND C1.course = 'UNIX基础'), 'o', 'x'),
       if((SELECT count(*) FROM Courses C1 WHERE C0.name = C1.name AND C1.course = 'Java中级'), 'o', 'x')
FROM (SELECT DISTINCT name FROM Courses) C0;

-- CASE表达式
SELECT name,
       IF(SUM(IF(course = 'SQL入门', 1, 0)) = 1, 'o', 'x')  AS 'SQL',
       IF(SUM(If(course = 'UNIX基础', 1, 0)) = 1, 'o', 'x') AS 'UNIX',
       IF(SUM(IF(course = 'java中级', 1, 0)) = 1, 'o', 'x') AS 'JAVA'
FROM Courses
GROUP BY name;
```

