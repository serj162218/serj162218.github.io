---
title: '110. Balanced Binary Tree'
date: 2023-01-12 17:18:16
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/description/)

### Tips:
- The advanced problem of maxDepth.
- if abs(left depth - right depth) > 1, means the tree is not height-balanced.
- if left or right < 0, means there has a subtree not equal as height-balanced.
- root can be null and it's still a height-balanced tree, so the depth can >= 0.
### Code:
```python=
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        return self.helper(root) >= 0
        
    def helper(self, root) -> int:
        if root == None:
            return 0

        left, right = self.helper(root.left), self.helper(root.right)

        if left < 0 or right < 0 or abs(left - right) > 1:
            return -1
        return max(left, right)+1
```