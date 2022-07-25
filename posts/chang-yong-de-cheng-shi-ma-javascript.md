---
title: '常用的程式碼 - Javascript'
date: 2022-07-25 14:31:42
tags: [常用的程式碼]
published: true
hideInList: false
feature: 
isTop: false
---
複製到剪貼簿
```js=
    <div onclick="copy(this)">text for copying.</div>
    function copy(element) {
        console.log($(element));
        let str = $(element).text();
        navigator.clipboard.writeText(str).then(() =>
            alert("copied");
        );
    };
```
