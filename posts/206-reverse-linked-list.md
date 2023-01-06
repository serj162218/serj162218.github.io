---
title: '206. Reverse Linked List'
date: 2023-01-03 12:12:27
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/)

Introduce:
> Given the head of a singly linked list, reverse the list, and return the reversed list.

Example 1:
![](https://i.imgur.com/3iCPy0G.png)

> Input: head = [1,2,3,4,5]
> Output: [5,4,3,2,1]

Example 2:
![](https://i.imgur.com/Jptx0gG.png)

> Input: head = [1,2]
> Output: [2,1]

Example 3:

> Input: head = []
> Output: []


> Constraints:
> The number of nodes in the list is the range [0, 5000].
> -5000 <= Node.val <= 5000


---

tips:
> curr to record the old linkedlist's pointer.
> prev to record the new linkedlist's pointer.
> next to record curr's next so the old pointer won't be lost.

| prev | curr | next |
|-|-|-|
|null|1|2|
|1|2|3|
|2|3|4|
|3|4|5|
|4|5|null|
|5|null|X|


```javascript=
var reverseList = function(head) {
    let prev, next, curr;
    prev = null;
    curr = head;
    while(curr){
        //save curr.next
        next = curr.next;
        //curr.next point to prev
        curr.next = prev;
        
        prev = curr;
        curr = next;
    }
    return prev;
};
```