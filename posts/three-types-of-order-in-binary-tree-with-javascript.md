---
title: 'Three types of Order in Binary Tree with Javascript ( Iteration )'
date: 2022-10-13 17:57:24
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
the algorithm is reference by https://www.youtube.com/watch?v=WLvU5EQVZqY
```javascript=
function TreeNode(val, left, right) {
    this.val = (val === undefined ? 0 : val)
    this.left = (left === undefined ? null : left)
    this.right = (right === undefined ? null : right)
}

var order = function (root) {
    if (!root) return [];
    let h = root;
    let ans = [];
    let stack = [];
    let map = new Map();
    do {
        if(h && map.get(h)){
            map.set(h, map.get(h)+1);
        }else if(h){
            map.set(h, 1);
        }

        if(map.get(h) === 1){ // 1 = preorder, 2 = inorder, 3 = postorder
            ans.push(h);
        }

        if(!h){
            h = stack.pop();
        }
        else if (h.left !== -1) {
            stack.push(h);
            let b = h.left;
            h.left = -1;
            h = b;
        } else if (h.right !== -1) {
            stack.push(h);
            let b = h.right;
            h.right = -1;
            h = b;
        } else if (h.left === -1 && h.right === -1){
            h = stack.pop();
        }
    } while (stack.length || h === root);
    ans = ans.map(v => v.val);
    return ans;
};
```