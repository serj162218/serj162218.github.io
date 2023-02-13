---
title: '875. Koko Eating Bananas'
date: 2023-01-13 15:43:13
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/description/)

### Tips:
- From sum the piles into one pile then divided by h, we will get the minimum rate L.
- When piles's amount is equal to h, the maximum rate is R.
- L ~ R, need to find the minimum rate, so it's L <= R.
- ans will be the minimum, so there's no need to use min(ans, k)
### Code:
```python=
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        L, R = math.ceil(sum(piles)/h), max(piles)
        ans = R

        while L <= R:
            k = ((L+R)//2)
            hour = sum(math.ceil(i/k) for i in piles)
            
            if hour > h:
                L = k + 1
            else:
                ans = k
                R = k - 1
        
        return ans
```