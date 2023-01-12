---
title: '226. Invert Binary Tree'
date: 2023-01-12 17:17:06
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/)

### Tips:
- dfs with reverting the two node (left and right).
- Remember to return the node.
### Code
```python=
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if root == None:
            return
        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)
        return root
```