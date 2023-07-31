---
title: 'vim 指南'
date: 2022-11-01 12:07:57
tags: [others]
published: true
hideInList: false
feature: 
isTop: false
---
insert mode
i
normal
esc
左下上右
h j k l
移動到下個單字
w
移動到此字的首
b
移動到此字的尾
e

數字+指令 = 重複指令三次
3h -> 往左三下 3w -> 跳三次下個單字

insert mode也能配數字
3igo esc -> gogogo
30i- esc -> 30個-

找下(上)個字，可以配數字 (單行搜尋)
f F
fo -> 找下個o
Fo -> 找上個o
3fq -> 往下找第三個q

找( { [ 配對的括弧
%

到句子首尾
0 $

跳到這個字的下(上)個地方 例如在the用*就會到下個the
* #

到檔案一開始(最後面)
gg G

到特定的行數 行數+G
2G -> 第二行

搜尋字 (往下 往上)
/ n N
例如 / text 搜尋text 然後往下(n)

insert new line(往下 往上) 會變insert mode
o O

delete word
x X

replace one word
r + 要的字

delete command
d
dw 刪掉下個單字
p可以貼上刪掉的字
d2e 可以配數字刪掉兩個字

重複上個指令
.
d2w後用 . 可以一直執行 d2w

visual mode (選取模式)
v
ex: v + e + l + d 刪掉目前這個字

:w (save) :q (quit) :q! (quit without saving)

u undo ctrl + R redo