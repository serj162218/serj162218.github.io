---
title: '150. Evaluate Reverse Polish Notation'
date: 2022-12-08 14:17:54
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

introduce:
Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, and /. Each operand may be an integer or another expression.

Note that division between two integers should truncate toward zero.

It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.

Example 1:
> Input: tokens = ["2","1","+","3","*"]
> Output: 9
> Explanation: ((2 + 1) * 3) = 9

Example 2:

> Input: tokens = ["4","13","5","/","+"]
> Output: 6
> Explanation: (4 + (13 / 5)) = 6

Example 3:

> Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
> Output: 22
> Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
> = ((10 * (6 / (12 * -11))) + 17) + 5
> = ((10 * (6 / -132)) + 17) + 5
> = ((10 * 0) + 17) + 5
> = (0 + 17) + 5
> = 17 + 5
> = 22

---
![](https://i.imgur.com/SLOKPY4.png)

```javascript=
var evalRPN = function (tokens) {
    let numArr = [];
    for (let i of tokens) {
        switch (i) {
            case "+":
                numArr.push(parseInt(numArr.pop() + numArr.pop()));
                break;
            case "-":
                //[3,4] => 3-4 => -4+3 = -1
                numArr.push(parseInt(-numArr.pop() + numArr.pop()));
                break;
            case "*":
                numArr.push(parseInt(numArr.pop() * numArr.pop()));
                break;
            case "/":
                //[2,5]=>2/5, so we need to use variable.
                let numB = numArr.pop();
                let numA = numArr.pop();
                //not using Math.floor() because 1/(-2) = -1, not we want.
                numArr.push(parseInt(numA / numB));
                break;
            default:
                numArr.push(parseInt(i));
                break;
        }
    }
    return numArr[0];
};
```