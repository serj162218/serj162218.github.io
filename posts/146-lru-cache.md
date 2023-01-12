---
title: '146. LRU Cache'
date: 2023-01-09 16:17:09
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[146. LRU Cache](https://leetcode.com/problems/lru-cache/)

tips:
- Make a Double LinkedList {next, prev, val, key} and a hashmap {key: Node}
- On the left side is the least recently use, and on the right side is the most recently use.
- put: remove the node first, then insert it. If len(hashmap) is greater than size, remove the leftest side Node 
- get: remove the Node then insert it on the rightest.

```python=
class Node:
    def __init__(self, val, key):
        self.val, self.key = val, key
        self.prev, self.next = None, None


class LRUCache:

    def __init__(self, capacity: int):
        self.cap = capacity
        # least recently use node | most recently use node
        self.left, self.right = Node(0, 0), Node(0, 0)
        self.cache = {}
        self.left.next, self.right.prev = self.right, self.left
    # Remove the LRU node
    def remove(self, node):
        nxt, prev =  node.next, node.prev
        nxt.prev, prev.next = prev, nxt

    # Insert to the MRU node
    def insert(self, node):
        prev, nxt = self.right.prev, self.right
        prev.next, nxt.prev = node, node
        node.prev, node.next = prev, nxt

    def get(self, key: int) -> int:
        if key in self.cache:
            self.remove(self.cache[key])
            self.insert(self.cache[key])
            return self.cache[key].val
        return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.remove(self.cache[key])
        
        self.cache[key] = Node(value, key)
        self.insert(self.cache[key])

        if len(self.cache) > self.cap:
            lru = self.left.next
            self.remove(lru)
            self.cache.pop(lru.key, None)
            
```