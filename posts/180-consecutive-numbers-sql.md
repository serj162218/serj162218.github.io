---
title: '180. Consecutive Numbers (SQL)'
date: 2023-02-13 11:13:33
tags: [sql,leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
# 180. Consecutive Numbers (SQL)
###### tags: `leetcode`, `sql`

[180. Consecutive Numbers](https://leetcode.com/problems/consecutive-numbers/description/)

### Tips:
1. use ROW_NUMBER OVER(ORDER BY num, id) to rank the num, num must be ordered first.
2. Because ROW_NUMBER() is stored by UNSIGNED, it will cause negative number in (id - ROW_NUMBER()). To avoid this, using CAST(... as SIGNED) to change the datatype is a good option.
3. By changing the num at "HAVING COUNT(\*) >= 3", it can be dynamic.

### Code:
```sql=
SELECT DISTINCT num as ConsecutiveNums 
FROM (
    SELECT num,
     id - CAST(ROW_NUMBER() OVER(ORDER BY num, id) as SIGNED) as rk
      FROM Logs
      ) u 
      GROUP BY u.num, u.rk 
HAVING COUNT(*) >= 3
```

```sql=
 -- No flexible.
 SELECT m.num as ConsecutiveNums FROM Logs m
     INNER JOIN Logs a ON a.num = m.num
     INNER JOIN Logs b ON b.num = m.num
 WHERE a.id = m.id + 1 AND b.id = m.id + 2
 GROUP BY m.num
```