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