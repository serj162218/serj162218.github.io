---
title: '104. Maximum Depth of Binary Tree'
date: 2023-01-12 17:17:40
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

### Tips:
- dfs until the node is None, then return 0
- Return the bigger depth between left node and right node with plus 1.
- This kind of problem is very basic. The method will be used in many questions.
### Code:
```python=
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if root == None:
            return 0
        maxDepth = max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
        return maxDepth
```