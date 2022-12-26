---
title: '416.partition-equal-subset-sum'
date: 2022-12-07 11:09:45
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[416.partition-equal-subset-sum](https://leetcode.com/problems/partition-equal-subset-sum/description/)

introduce:
Given a non-empty array nums containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

Example 1:
> Input: nums = [1,5,11,5]
> Output: true
> Explanation: The array can be partitioned as [1, 5, 5] and [11].

Example 2:

> Input: nums = [1,2,3,5]
> Output: false
> Explanation: The array cannot be partitioned into equal sum subsets.

---

Because the array with be partitioned into two parts, we only need to find the target which is sum(nums)/2.
> note: sum(nums) is always a even number.

Then there's two way:
1. 
![](https://i.imgur.com/nHqlPIQ.png)


2. 
![](https://i.imgur.com/FnOz6Gg.png)

---

1. Set, not efficient.
![](https://i.imgur.com/vWkJlUc.png)


```javascript=
var canPartition = function (nums) {
    let sum = nums.reduce((a, b) => a + b);
    if (sum % 2 == 1) return false;
    
    let target = sum / 2;
    let dp = new Set();
    
    dp.add(0);
    
    for (let i = 0; i < nums.length; i++) {
        let nextDP = new Set(dp);
        
        for (let j of new Set(nextDP)) {
            if (j + nums[i] > target) continue;
            if (j + nums[i] === target) return true;
            nextDP.add(j + nums[i]);
        }
        dp = nextDP;
    }
    
    return dp.has(target) ? true : false;
};
```

2. Array, still not efficient.
> Similar with using set
> Because using array will iterate every element, so it must add if(dp[key] === true)
> Still not good because the loop at Line.11, array need to deep copy before it.
```javascript=
var canPartition = function (nums) {
    let sum = nums.reduce((a, b) => a + b);
    if (sum % 2 == 1) return false;

    let target = sum / 2;
    let dp = Array(target + 1).fill(false);

    dp[0] = true;
    for (let i = 0; i < nums.length; i++) {
        let nextDP = [...dp];
        for (let key = 0; key < nextDP.length; key++) {
            if (dp[key] === true) {
                nextDP[parseInt(key) + nums[i]] = true;
            }
        }
        dp = nextDP;
    }
    return dp[target] === true;
};
```


3. Array (the most efficiently )

> Line 10: using j is start at target, the idea is to fix the problem below(need to copy every time before it starting to loop)
> j is always need to bigger than nums[i] so it won't be negative.
![](https://i.imgur.com/RVk32Om.png)

```javascript=
var canPartition = function (nums) {
    let sum = nums.reduce((a, b) => a + b);
    if (sum % 2 == 1) return false;

    let target = sum / 2;
    let dp = Array(target + 1).fill(false);

    dp[0] = true;
    for (let i = 0; i < nums.length; i++) {
        for (let j = target; j >= nums[i]; j--) {
            dp[j] = dp[j - nums[i]] || dp[j];
        }
        if (dp[target] === true) return true;
    }
    return dp[target] === true;
};
```

---

1. using Set (Slower than using Array, because by using iterator, it need to deep copy the set first. By doing this, when it is iterating, the origin set won't be effected.)
![](https://i.imgur.com/T8PUAZL.png)

```javascript=
var canPartition = function (nums) {
    let sum = nums.reduce((a, b) => a + b);
    if (sum % 2 == 1) return false;

    let target = sum / 2;
    let dp = new Set();

    dp.add(target);

    for (let i of nums) {
        let nextDP = dp;
        for (let j of new Set(nextDP)) {
            if (j >= i)
                nextDP.add(j - i);
        }
        if (dp.has(0)) return true;
    }
    return dp.has(0);
};
```