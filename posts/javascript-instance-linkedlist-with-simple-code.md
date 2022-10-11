---
title: 'Javascript instance LinkedList with simple code'
date: 2022-10-11 14:13:55
tags: [program]
published: true
hideInList: false
feature: 
isTop: false
---
```javascript=
class LinkedList {

    constructor() {
        this.node = new ListNode(-1);
        this.length = 0;
    }

    /**
     * Append a new ListNode to this LinkedList
     */
    append(val) {
        let head = this.node;
        while (head.next) {
            head = head.next;
        }
        head.next = new ListNode(val);
        this.length++;
    }

    /**
     * delete the first item which's value == val
     */
    delete(val) {
        let head = this.node.next;
        let prev = this.node;
        while (head) {
            if (head.val === val) {
                prev.next = head.next;
                this.length--;
                break;
            }
            prev = head;
            head = head.next;
        }
    }

    /**
     * insert a node after specified position.
     */
    insert(val, index = this.length - 1) {
        if (index > this.length) {
            console.log(`index ${index} is bigger than this list's length ${this.length}.`);
        }
        let count = index + 1;
        let head = this.node;
        while (count !== 0) {
            head = head.next;
            count--;
        }
        head.next = new ListNode(val, head.next);
        this.length++;
    }

    /**
     * Print all the values in this LinkedList
     */
    print() {
        let head = this.node.next;
        while (head) {
            console.log(head.val);
            head = head.next;
        }
    }
}
class ListNode {
    constructor(val, next) {
        this.val = val ? val : null;
        this.next = next ? next : null;
    }
}

let a = new LinkedList;
a.append(1);
a.append(2);
a.append(3);
a.print(); // 1,2,3
a.delete(3);
a.insert(4, 0);
a.insert(5);
a.print(); //1, 4, 2, 5
console.log(a.length); //4
```

