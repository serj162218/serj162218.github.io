---
title: '22. Generate Parentheses'
date: 2022-12-09 14:15:59
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

Introduce:
> Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

Example 1:

> Input: n = 3
> Output: ["((()))","(()())","(())()","()(())","()()()"]

Example 2:

> Input: n = 1
> Output: ["()"]

 

> Constraints:
>     1 <= n <= 8

---

tips:
* Because n defined the amount of pairs, n\*2 is equal to the string's length.
* The left parenthesis amount is always equal or bigger than the right parenthesis amount.
* Using dfs to try every sets which is in the condition above.

```javascript=
var generateParenthesis = function (n) {
    let dfs = (n, str, open, close) => {
        if (str.length === n * 2) {
            ans.push(str);
            return true;
        }

        if (open < n)
            dfs(n, str + "(", open + 1, close);

        if (open > close)
            dfs(n, str + ")", open, close + 1);

    }
    let ans = [];
    dfs(n, "", 0, 0);
    return ans;
};
```