---
title: MySQL之You can't specify target table for update in FROM clause解决办法
date: 2018-12-6
categories: mysql
tags: 
- mysql
- 实用代码片段
---

mysql并不支持对同一表的数据作为同一表的更新数据.

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

但是,像是这样,却是可以运行的
```sql
UPDATE vip_member
SET dynamic_reward = (SELECT ifnull(sum(times * least(left_dynamic_reward, right_dynamic_reward) * 2 * 0.03 * 0.95), 0)
                      FROM vip_dynamic_reward
                      WHERE vip_member.id = vip_dynamic_reward.member_id);
```

两者的差距看起来就是update/delete之后没有紧跟着where.事实上可以运行的那一句在运行时会提醒` Unsafe query: 'Update' statement without 'where' updates all table rows at once`