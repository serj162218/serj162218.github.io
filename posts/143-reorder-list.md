---
title: '143. Reorder List'
date: 2023-01-06 13:37:53
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[143. Reorder List](https://leetcode.com/problems/reorder-list/description/)

Introduce:
> You are given the head of a singly linked-list. The list can be represented as:
> L0 → L1 → … → Ln - 1 → Ln
> 
> Reorder the list to be on the following form:
> L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
> 
> You may not modify the values in the list's nodes. Only nodes themselves may be changed.


Example 1:
![](https://assets.leetcode.com/uploads/2021/03/04/reorder1linked-list.jpg)
> Input: head = [1,2,3,4]
Output: [1,4,2,3]

Example 2:
![](https://assets.leetcode.com/uploads/2021/03/09/reorder2-linked-list.jpg)
> Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]

 

 

Constraints:
- The number of nodes in the list is in the range [1, 5 * 104].
- 1 <= Node.val <= 1000


---

tips:
- If it's an array, you can use two pointer with L and R to direct order the element. But it's LinkedList now, so you can't left shift R.
- In this case, you can first reverse the LinkedList which is for R to traverse.
- First step, find the middle of LinkedList. Be careful for the fast pointer because we want to find the middle's previous point.
![](https://i.imgur.com/l7t27hn.png)
- Step 2: Reverse the m's next LinkedList.[1,2,4,3]
- To avoid the cycle in step 3(reorder), we will seperate the LinkedList into two pieces.[1,2] [4,3]
- Step 3: Reorder, not a big deal.

```python=
class Solution:
    def reorderList(self, head: ListNode) -> None:
        # find Middle
        fast, slow = head, head
        while fast.next and fast.next.next:
            fast = fast.next.next
            slow = slow.next

        # reverse middle
        curr, prev, next = slow.next, None, None
        while curr != None:
            next = curr.next
            curr.next = prev
            prev = curr
            curr = next

        
        #break the linkedlist into two piece to prevent cycle in reorder
        slow.next = None

        # reorder
        L, R = head, prev
        while L and R:
            tmpL = L.next
            tmpR = R.next

            L.next = R
            L = tmpL

            R.next = L
            R = tmpR
```