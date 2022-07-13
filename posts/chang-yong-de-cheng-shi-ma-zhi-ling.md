---
title: '常用的程式碼 - 指令'
date: 2022-07-13 11:38:45
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
ps aux|grep master //查所有socket的執行緒
ss -nlp|grep 5000 //5000是port 查對應的pid用
kill -15 {pid} 砍pid的程式，會跟著砍子程式，程式當機會砍不掉
kill -9 {pid} 強制砍pid的程式，子程式的父pid會變1
php server_global_customer.php 可以開socket
crontab -e 編輯crontab
-l 看crontab
-r 全部刪除

查檔案
find . -name "*.txt"

kill -USR1 {pid} 熱重啟(socket主程式修改的話要砍掉重開)
tail -f swoole.log 一直偵測log的最新訊息
tail -f swoole.log | grep "關鍵字" 偵測關鍵字
cat swoole.log | grep "關鍵字" 印出所有有關鍵字的
echo > swoole.log 清空log
cat swoole.log | grep "關鍵字" -C 5 印出關鍵字與上下五行
cat swoole.log | grep "關鍵字" > text.log 把log寫進text.log

git config --global  --list

grep -Frnw '.' -e '登入頁面' 目錄中找關鍵字
-F 用簡單的字串查詢
-r recursive
-n 列出Line
-w 字串全部對上才會顯示

netstat -tulpn | grep LISTEN 查PORT+PID