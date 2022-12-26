---
title: 'Three types of Order in Binary Tree with Javascript ( Recursion )'
date: 2022-11-07 16:59:26
tags: [program]
published: true
hideInList: false
feature: 
isTop: false
---
```javascript=
/**
 * Three order method in tree
 * the key is "arr.push(node.val)"
 * preorder = do the things first.
 * inorder = do it in the middle.
 * postorder = do it last.
 * 
 * This example is postorder
 */
var OrderTraversal = function (root) {
    let arr = [];
    let helper = function (node) {
        if (!node) { return false; }
        helper(node.left);
        helper(node.right);
        arr.push(node.val);
    }
    helper(root);
    return arr;
};
```