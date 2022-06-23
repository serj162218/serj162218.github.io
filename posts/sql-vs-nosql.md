---
title: 'SQL vs NoSQL'
date: 2022-06-23 15:03:57
tags: [program]
published: true
hideInList: false
feature: 
isTop: false
---
<span class="subtitle">前言</span> <span class="content">個人隨筆，簡單紀錄一下會用到的時機</span>

---
SQL = RDBMS，關聯式資料庫
NoSQL 就是相反的非關聯式資料庫

SQL可以保證資料的完整性，永久性，還可以保證獨一性，對於需要用到有互相關連的資料會比較適合，像是作者跟文章，然後這作者又有自己的設定檔、簡介等等情況，用SQL會比較方便。
SQL的Schema需要先設計好，不然之後要更改的時候如果有資料了會很麻煩，因為資料型態要一樣。
不好用於分散式資料庫，因為不同資料彼此之間可能會有關聯，如果分散在不同資料庫，查詢起來會很麻煩，通常要升級效能的話直接升級伺服器會比較好（vertical scaling）。

NoSQL的話就像一堆文件存在伺服器內，每個文件的資料格式不用一樣，目前還沒有遇到會想使用NoSQL的情況，用於分散式資料庫會比較方便（horizontal scaling），畢竟都是存文件，也不會需要注意一致性跟完整性的問題，所以與其升級一台主機的效能，不如多開幾台幫忙存就好了。