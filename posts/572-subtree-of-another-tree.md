---
title: '572. Subtree of Another Tree'
date: 2023-01-12 17:18:48
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[572. Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/)

## Dfs compare Method
### Tips:
- The advanced problem of Same Tree.
- Both of root and subroot won't be null.
- Keep tracking the root's node until it's null, then return False. It means none of the root's subtree is same as subRoot's subtree.
- Do not combine Line.6~Line.9 because it will waste more efficiency.
- Time complexity O(m\*n)
### Code:
```python=
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        if not root:
            return False
            
        elif self.isSameTree(root, subRoot):
             return True
        else:
            return self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot)

    def isSameTree(self, p, q):
        if p == None or q == None:
            return p == q
        
        if p.val == q.val:
            return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
        else:
            return False
```

## String compare Method

### Tips:
- This idea is convert all the node's val into string. Then compare if subRoot's str is in root's str.
- It's more faster than below's method, the time complexity is O(m+n) which m = root's node total amounts and n = subroot's node total amounts.
### Code:
```python=
class Solution:
    def isSubtree(self, root, subRoot) -> bool:
        rootStr = self.traverse(root)
        subStr = self.traverse(subRoot)
        return subStr in rootStr

    def traverse(self, node) -> str:
        if node:
            return f"^{node.val} {self.traverse(node.left)} {self.traverse(node.right)}"
        return None
```