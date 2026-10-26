# SQL：GROUP BY + HAVING

## 基础：分组统计

```sql
SELECT user_id, COUNT(*) AS cnt, SUM(amount) AS total
FROM orders
GROUP BY user_id;
```

## WHERE vs HAVING

```sql
SELECT user_id, SUM(amount) AS total
FROM orders
WHERE created_at >= '2026-01-01'  -- 分组前过滤：行级
GROUP BY user_id
HAVING total > 1000;              -- 分组后过滤：组级
```

- WHERE：分组前筛**行**，能用索引。
- HAVING：分组后筛**组**，筛的是聚合结果。

## 查"下单超过 3 次的用户"

```sql
SELECT user_id, COUNT(*) AS cnt
FROM orders
GROUP BY user_id
HAVING cnt > 3;
```

## 坑

1. SELECT 里非聚合列必须出现在 GROUP BY 里
   （MySQL 宽松但结果随机，别依赖）。
2. HAVING 里可以用 SELECT 定义的别名，WHERE 不行
   （执行顺序：WHERE → GROUP BY → HAVING → SELECT）。
3. 数据量大时先 WHERE 缩小范围再分组，别全表 GROUP。

记住执行顺序，WHERE/HAVING 就不会用反。
