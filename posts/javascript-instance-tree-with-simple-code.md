---
title: 'Javascript instance Binary Tree with simple code'
date: 2022-10-11 16:07:31
tags: []
published: false
hideInList: false
feature: 
isTop: false
---
- Every node can only have at most 2 children.

This tree is imcompleted because it doesn't have delete function and auto-balance function.
```javascript=
class Tree {
    constructor(val) {
        this.root = new Heap(val);
    }
    add(val, heap = this.root) {
        if (val >= heap.val) {
            if (heap.right !== null) {
                this.add(val, heap.right);
            } else {
                heap.right = new Heap(val);
            }
        } else {
            if (heap.left !== null) {
                this.add(val, heap.left);
            } else {
                heap.left = new Heap(val);
            }
        }
    }
}

class Heap {
    constructor(val, left, right) {
        this.val = val ? val : null;
        this.left = left ? left : null;
        this.right = right ? right : null;
    }
}

let tree = new Tree(5);
tree.add(3);
tree.add(7);
tree.add(8);
tree.add(4);
console.log(tree);
```