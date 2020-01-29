---
title: MySQL提示1418错误
date: 2020-1-8
categories: mysql
tags: 
- mysql
---

MySQL提示1418错误
```
ERROR 1418 (HY000): This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)
```
这样就行啦
```sql
set global log_bin_trust_function_creators=1;
```
