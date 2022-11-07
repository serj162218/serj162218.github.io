---
title: 'Binary Tree Preorder Traversal with Javascript ( Iteration )'
date: 2022-10-12 17:20:24
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
Preorder, the first time pointer meet the node, print it out.

```javascript=
//structor
function TreeNode(val, left, right) {
     this.val = (val===undefined ? 0 : val)
     this.left = (left===undefined ? null : left)
     this.right = (right===undefined ? null : right)
 }

var preorderTraversal = function(root) {
    if(!root) return [];
    let ans = [];
    let h = root;
    
    let stack = []; //to remember the previous node
    while(h){
        if(!ans.includes(h)){
            ans.push(h); // not h.val because h.val may be duplicated.
        }
        if(h.left !== null && !ans.includes(h.left)){
            stack.push(h);
            h = h.left;
        }else if(h.right !== null && !ans.includes(h.right)){
            stack.push(h);
            h = h.right;
        }else{
            h = stack.pop();
        }
    }

    //switch h to h.val
    ans = ans.map(v => v.val);
    
    return ans;
};
```