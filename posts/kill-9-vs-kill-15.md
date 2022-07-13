---
title: 'kill -9 v.s. kill -15'
date: 2022-07-11 16:43:13
tags: [program]
published: true
hideInList: false
feature: 
isTop: false
---

<span class="subtitle">隨筆</span>
有遇過kill -15 PID 結果砍不掉的問題
但是程式整個卡住，無法確定是不是因為程式跑太久，或是有太多連線導致程式無法正常結束
此時就會用kill -9強制結束
但kill -9 無法砍掉子行程，會讓子行程的父PID變成1
如果kill -15不行，就-9把master砍掉，再把底下的子行程用-15砍掉，看能不能全砍

ps -ef | grep webServer 看這隻程式與子行程
所有人 PID 父PID(最大是1) unknown 執行時間 unknown 執行多久 後面參數不確定
ps aux | grep master 看所有master