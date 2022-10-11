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
```javascript=
    <div onclick="copy(this)">text for copying.</div>
    function copy(element) {
        console.log($(element));
        let str = $(element).text();
        navigator.clipboard.writeText(str).then(() =>
            alert("copied");
        );
    };
```
比直接用append更好的產生element的方式
```javascript=
let option = $("<option>", {"d-id":e['id'], text: e['name']});
$("#type").append(option);
```
ajax範例
```js=
 $.ajax({
            url: url,
            data,
            method: "GET",
            dataType: "json",
        }.done((rs) => {

        }).fail((rs) => {

        })
```
ajax會重複用的話可以變成一個新的function
```js=
    let callAjax = (data, url, method, dataType) => {
        return $.ajax({
            url,
            data,
            method,
            dataType,
        });
    }
    callAjax(data, url, method, dataType).done()...
```

Array common function

```javascript=
//add vs delete
push()  pop() //latest element in this array
unshift()  shift() //first element in this array

splice(index, nums) //remove the specified position element with multiple amount.

reverse() //reverse this array.

arr1.concat(arr2) // return a new array with two arrays combination, it won't change neither arr1 nor arr2.

include(val) //return true if array contains this value, else return false

indexOf(val) //like include, but return the position, or -1

join(', ') //print out the element as string, split with argument
```

