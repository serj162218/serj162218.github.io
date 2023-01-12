---
title: '543. Diameter of Binary Tree'
date: 2023-01-12 17:17:57
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/description/)

### Tips:
- The advanced problem of maxDepth.
- Use a global var to keep the max diameter, Then return the max depth between left and right nodes.
- If node is null, return -1 because the leaf's diameter is 0, not 1.
### Code:
```python=
class Solution:
    def __init__(self):
        self.maxLen = 0

    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        self.helper(root)
        return self.maxLen

    def helper(self, root) -> int:
        if root == None:
            return -1
        left = self.helper(root.left) + 1
        right = self.helper(root.right) + 1
        self.maxLen = max(self.maxLen, left + right)
        return max(left, right)
```