---
title: '常用的程式碼 - MySQL'
date: 2022-07-13 11:49:01
tags: [常用的程式碼]
published: true
hideInList: false
feature: 
isTop: false
---
創建新欄位
在TABLE內新增欄位
ALTER TABLE 資料表名稱 ADD COLUMN 欄位名稱 形態(長度);
ALTER TABLE member ADD COLUMN tg_id int(64) NOT NULL AFTER FB_id, ADD INDEX(tg_id);
在TABLE內新增欄位，欄位必須加入INDEX並且設定DEFAULT值
ALTER TABLE 資料表名稱 ADD COLUMN 欄位名稱 形態(長度), ADD INDEX(欄位名稱);


刪除所有問號
UPDATE `test` SET `content`= REPLACE(content, '?', '') WHERE `content` LIKE "%?%"

正規表達式
UPDATE `test` SET `msg`=REGEXP_REPLACE(msg,'(.*)( $)','\\1') WHERE msg REGEXP '.* $'

查所有外來鍵
SELECT * FROM information_schema.TABLE_CONSTRAINTS WHERE CONSTRAINT_TYPE = "FOREIGN KEY" AND CONSTRAINT_SCHEMA = "test"

叢集+GROUP BY
SELECT name, subject FROM `student`, score GROUP BY name, subject

範例 拿缺考的學生
SELECT name, subject FROM `student`, score WHERE (subject, name) NOT IN ( SELECT s.subject as subject, c.name as name FROM score s LEFT JOIN student c ON c.id = s.id) GROUP BY name, subject

排名 DENSE_RANK()
SELECT score, DENSE_RANK() OVER (
    ORDER BY score DESC
) `rank`
FROM Scores ORDER BY score DESC;

取第二高的薪水，沒有的話顯示null
SELECT CASE WHEN COUNT(b.c) > 0 THEN b.c ELSE null END AS SecondHighestSalary
FROM (SELECT DISTINCT salary as c FROM Employee ORDER BY salary DESC LIMIT 1,1) b;

//看別人的，優化版
SELECT (CASE 
        WHEN (SELECT COUNT(DISTINCT Salary) FROM Employee) < 2 
        THEN NULL 
        ELSE (SELECT Salary FROM Employee 
              ORDER BY Salary DESC LIMIT 1,1) 
        END) AS SecondHighestSalary

跳過前3筆 OFFSET
SELECT DISTINCT(salary) from Employee order by salary DESC LIMIT 1 OFFSET 3