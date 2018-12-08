---
title: MySQL之You can't specify target table for update in FROM clause解决办法
date: 2018-12-6
categories: mysql
tags: 
- mysql
- 实用代码片段
---

mysql并不支持对同一个表"同时"做查询和删除操作

如
```sql
DELETE
FROM vip_reward_record
WHERE type = 1
  AND id NOT IN (SELECT max(id) FROM vip_reward_record WHERE type = 1 GROUP BY member_id);
```
这样就中提示 `You can't specify target table for update in FROM claus`

但解决也不难
在select外边套一层，让数据库认为你不是查同一表的数据作为同一表的更新数据,就向像这要"包装"一层就可以了

```sql
DELETE
FROM vip_reward_record
WHERE type = 1
  AND id NOT IN (SELECT id FROM (SELECT max(id) AS id FROM vip_reward_record WHERE type = 1 GROUP BY member_id) a);
```