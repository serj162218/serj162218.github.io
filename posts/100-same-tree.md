---
title: '100. Same Tree'
date: 2023-01-12 17:18:32
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[100. Same Tree](https://leetcode.com/problems/same-tree/description/)

### Tips:
- if p or q is null, check if both of them are null.
- check (every two nodes's val) and (their left and right nodes).
- This kind of problem is very basic. The method will be used in many questions.
### Code:
```python=
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if p == None or q == None:
            return p == q
        return p.val == q.val and self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```