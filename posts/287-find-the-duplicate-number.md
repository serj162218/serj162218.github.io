---
title: '287. Find the Duplicate Number'
date: 2023-01-09 14:32:55
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/description/)

Introduce:
> Given an array of integers nums containing n + 1 integers where each integer is in the range [1, n] inclusive.
There is only one repeated number in nums, return this repeated number.
You must solve the problem without modifying the array nums and uses only constant extra space.

Example 1:

> Input: nums = [1,3,4,2,2]
Output: 2

Example 2:

> Input: nums = [3,1,3,4,2]
Output: 3


Constraints:
- 1 <= n <= 105
- nums.length == n + 1
- 1 <= nums[i] <= n
- All the integers in nums appear only once except for precisely one integer which appears two or more times.



---

tips:
- This is a classic problem with Floyd Cycle Detection Algorithm.
- First, find the node M which fast and slow is the same.
- Second, one pointer starts from beginning, anothoer pointer starts from node M, both of pointers move one by one step until they are point the same node.

> If you want to know the cycle length, in the node M, let slow pointer move one by one step until it point to the fast pointer.

```python=
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        slow, fast = nums[0], nums[0]
        while True:
            fast = nums[nums[fast]]
            slow = nums[slow]
            if nums[slow] == nums[fast]:
                break

        p = nums[0]
        while p != slow:
            p = nums[p]
            slow = nums[slow]

        return p
```