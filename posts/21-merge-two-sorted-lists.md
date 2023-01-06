---
title: '21. Merge Two Sorted Lists'
date: 2023-01-04 14:07:35
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/description/)

Introduce:
> You are given the heads of two sorted linked lists list1 and list2.
> Merge the two lists in a one sorted list. The list should be made by splicing together the nodes of the first two lists.
> Return the head of the merged linked list.

Example 1:
![](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)
> Input: list1 = [1,2,4], list2 = [1,3,4]
> Output: [1,1,2,3,4,4]

Example 2:

> Input: list1 = [], list2 = []
Output: []

Example 3:

> Input: list1 = [], list2 = [0]
Output: [0]


> Constraints:
> The number of nodes in both lists is in the range [0, 50].
> -100 <= Node.val <= 100
> Both list1 and list2 are sorted in non-decreasing order.


---

tips:
* Make a new List to keep the head for return, then a pointer for merging.
* When list1 or list2 is null, let the merge pointer's next point to the remaining one.


```javascript=
var mergeTwoLists = function(list1, list2) {
    let merge = new ListNode();
    let curr = merge;
    while(list1 !== null && list2 !== null){
        if(list1.val < list2.val){
            curr.next = list1;
            list1 = list1.next;
        }else{
            curr.next= list2;
            list2 = list2.next;
        }
        curr = curr.next;
    }
    if(!list1) curr.next = list2;
    if(!list2) curr.next = list1;
    return merge.next;
}
```