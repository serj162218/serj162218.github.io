---
title: '常用的程式碼 - git'
date: 2022-07-13 11:42:03
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
git add --all :/ 加入所有有修改的檔案
(在根目錄 git add . 是一樣的效果)
git add -u 檔名有改/刪除的話用這個
git config --global --list
git reset HEAD filename 取消git add的檔案
git stash 暫存目前修改的狀態，然後還原成原本的樣子(untrach的file不會被暫存)
git stash pop 還原最新一筆stash
git stash list 列出所有暫存
git commit -m "註解" commit直接下註解