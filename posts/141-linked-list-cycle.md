---
title: '141. Linked List Cycle'
date: 2023-01-09 11:00:21
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

Introduce:
> Given head, the head of a linked list, determine if the linked list has a cycle in it.
> There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.
> Return true if there is a cycle in the linked list. Otherwise, return false.

Example 1:
![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)
> Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

Example 2:
![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)
> Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.

Example 3:
![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)
>Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.

> Constraints:
>     1 <= n <= 8

---

tips:
* Floyd Cycle Detection Algorithm
* Make two pointer 'fast' and 'slow'
* fast is always move by 2 steps, slow is always move by 1 step.
* If fast == slow means there's a cycle.
* Else there's no cycle.

```python=
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow, fast = head, head

        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if slow == fast:
                return True
        return False
```