---
title: '19. Remove Nth Node From End of List'
date: 2023-01-06 11:36:55
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)

Introduce:
> Given the head of a linked list, remove the nth node from the end of the list and return its head.

Example 1:
![](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

>Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]

Example 2:

>Input: head = [1], n = 1
Output: []

Example 3:

>Input: head = [1,2], n = 1
Output: [1]

Constraints:
> The number of nodes in the list is sz.
> 1 <= sz <= 30
> 0 <= Node.val <= 100
> 1 <= n <= sz

Follow up: Could you do this in one pass?

---
## Two Pointer Solution
tips:
1. Two pointer, one is shifting with n element first. Then the distance in fast pointer and slow pointer will be n.
2. if fast pointer is null, it means the element which need to be removed is the List's HEAD.
3. else, shift two pointer until one is null(it'll always be fast pointer). then the slow.next is the nth element.

```python=
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        fast, slow = head, head
        for _ in range(n):
            fast = fast.next

        if fast == None:
            return head.next
        
        while fast.next != None:
            fast = fast.next
            slow = slow.next

        slow.next = slow.next.next
        
        return head
```

## Index Solution (Easy to understand)

tips:
1. By using a global variable to record every element's position with desc. It will be easy to know which element is need to be removed.

```python=
class Solution:

    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        if head == None:
            self.index = 0
            return head
        
        head.next = self.removeNthFromEnd(head.next, n)
        self.index += 1
        return head if self.index != n else head.next
```