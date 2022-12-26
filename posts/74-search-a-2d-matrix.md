---
title: '74. Search a 2D Matrix'
date: 2022-12-26 15:00:45
tags: [leetcode]
published: true
hideInList: false
feature: 
isTop: false
---
###### tags: `leetcode`

[74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

Introduce:
> Write an efficient algorithm that searches for a value target in an m x n integer matrix matrix. This matrix has the following properties:
> - Integers in each row are sorted from left to right.
> - The first integer of each row is greater than the last integer of the previous row.


Example 1:

> Input: matrix = [[1,3,5,7],[10,11,16,20], [23,30,34,60]], target = 3
> Output: true

Example 2:

> Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
> Output: false

 

Constraints:
> - m == matrix.length
> - n == matrix[i].length
> - 1 <= m, n <= 100
> - -104 <= matrix[i][j], target <= 104


---

tips:
> - The last integer in every row is always less than the next row's first integer.
> - Because m and n could be a very big number, it's not a nice option to search every row and column (O(m\*n))
> - By the first tip, compare the first element in every row, then search the row which is the most possible with target.
> - The time complexity is O(logn) because it will execute at most two binary search.

```javascript=
let searchMatrix = (matrix, target) => {
    let L = 0, R = matrix.length - 1;
    let M;
    while (L < R) {
        M = ~~((L + R) / 2);
        if (M > 0 && matrix[M][0] > target && matrix[M - 1][0] < target) return searchValue(matrix[M - 1], target);
        else if (M < matrix.length - 1 && matrix[M][0] < target && matrix[M + 1][0] > target) return searchValue(matrix[M], target);

        if (matrix[M][0] > target) R = M;
        else if (matrix[M][0] < target) L = M + 1;
        else return searchValue(matrix[M], target);
    }
    return searchValue(matrix[R], target);
};

let searchValue = (arr, target) => {
    if (arr.length < 2) return arr[0] == target ? true : false;
    let L = 0, R = arr.length - 1;
    let M;
    while (L < R) {
        M = ~~((L + R) / 2);
        if (arr[M] > target) R = M - 1;
        else if (arr[M] < target) L = M + 1;
        else return true;
    }
    return arr[R] == target ? true : false;
}
```