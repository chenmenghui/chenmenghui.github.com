---
title: MySQL常见函数
date: 2019年1月26日
categories: 
- MySQL
tags: 
- MySQL
---

## 字符函数

1. length 获取参数值中的字节个数

select length('join'); // 4

2. concat 拼接字符串

3. upper lower 大小写转化

4. substr substring 截取从指定索引处后面所有字符(索引从1开始) 

5. instr 返回字符串字串第一次出现的索引

select instr('abcde', 'b');

6. trim

select trim ('a' from 'abcdea');

7. lpad rpad 用指定的字符串填充

select lpad('*', 10, 'abc');

8. replace 替换

select replace ('abcdf', 'f','e');

## 数学函数

1. round 四舍五入

select round(1.65);

select round(1.655, 2);

2. ceil 天花板数

3. floor 地板数

4. truncate 截断

select truncate(1.999, 2);

5. mod 取余

## 日期函数

1. now 返回当前系统日期时间

2. curdate 返回当前系统日期

3. curtime 返回当前的时间

4. year month day hour minute second 获取年月日时分秒

5. str_to_date 将日期格式的字符串转化成指定格式的时间

| 格式符 | 功能 |
|---|---|
| %Y | 四位数年份 |
| %y | 两位数年份 |
| %m | 月份(01,02,...12) |
| %c | 月份(1,2,...12) |
| %d | 日(01,02,...) |
| %H | 小时(24) |
| %h | 小时(12) |
| %i | 分钟 |
| %s | 秒 |

6. date_format 将日期转化成字符

## 其他函数

如version,user,database之类的

## 流程控制函数

1. if 

select if(10<5, 'y', 'n');

2. case

使用一(类似switch)
case 要判断的字段或表达式
when 常量 then 要显示的值或语句1;
when 常量 then 要显示的值或语句2;
...
else 都没有命中的时候
end;

select salary,department_id
case 1 then salary*1.1
case 2 then salary*1.2
else salary
end from emplory;

使用二(多重if)

case 
when 条件1 then 要显示的内容1
when 条件2 then 要显示的内容2

# 多行函数(聚合函数)

sum avg max min count

特点:
1. sum avg用于处理数值类型,max min count可以处理其他
2. 忽略null值
3. 可以和distinct搭配实现去重
4. count统计行数,count(*)效率较高
5. 和分组函数一同查询字段要求是group by之后的字段

```sql
# count函数详细介绍

SELECT count(salary) FROM employees;

SELECT count(*) FROM employees; # 和上着的区别在于,这个展示的是行数,上个取得是除null之后的数

select count(1) from employees; # 相当于数据多了一行1,然后统计个数(实际就是行数)
```
