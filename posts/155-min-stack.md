---
title: '155. Min Stack'
date: 2022-12-07 14:46:54
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[155. Min Stack](https://leetcode.com/problems/min-stack/description/)

introduce:
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the MinStack class:

    MinStack() initializes the stack object.
    void push(int val) pushes the element val onto the stack.
    void pop() removes the element on the top of the stack.
    int top() gets the top element of the stack.
    int getMin() retrieves the minimum element in the stack.

You must implement a solution with O(1) time complexity for each function.

Example 1:

> Input
> ["MinStack","push","push","push","getMin","pop","top","getMin"]
> [[],[-2],[0],[-3],[],[],[],[]]
> 
> Output
> [null,null,null,null,-3,null,0,-2]
> 
> Explanation
> MinStack minStack = new MinStack();
> minStack.push(-2);
> minStack.push(0);
> minStack.push(-3);
> minStack.getMin(); // return -3
> minStack.pop();
> minStack.top();    // return 0
> minStack.getMin(); // return -2

---

This question's point is below
- The time complexity must in O(1).
- It need to remember the minimum value in the stack.
- While popping the value, it can't be scanned again to get the minimum value, so the minimum value must be remembered while pushing.

I'm using a new constructor call "Node" to remember the minimum value for every node.
This stack is seems like a LinkedList.
```javascript=
class Node {
    constructor(val, min, next) {
        this.val = val;
        this.min = min;
        this.next = next;
    }
}
class MinStack {
    constructor() {
        this.head = new Node(null, null, null);
    }
    /**
     * @param {number} val
     * @return {void}
     */
    push(val) {
        this.head = new Node(val, Math.min(val, this.head.min===null?val:this.head.min), this.head);
    }
    /**
     * @return {void}
     */
    pop() {
        this.head = this.head.next;
    }
    /**
     * @return {number}
     */
    top() {
        return this.head.val;
    }
    /**
     * @return {number}
     */
    getMin() {
        return this.head.min;
    }
}
```

![](https://i.imgur.com/1f00igh.png)
