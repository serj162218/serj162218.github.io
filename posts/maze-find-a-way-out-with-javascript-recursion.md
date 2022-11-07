---
title: 'Maze find a way out with Javascript ( Recursion & Iteration )'
date: 2022-10-14 15:38:17
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
<h1>遞迴版本</h1>
從0,0開始往四個方向走，用一個array去存現在走哪個方向
如果碰到終點(maze.length-1 && maze.length -1)就代表成功了
如果有一條遞迴的路成功了，就一直把這成功的訊息傳下去
如果這條路失敗了，就把array剛剛存的「現在走的方向」移除掉
最後輸出array就是答案了
在走的時候，下一步不能是原點，也不能超出範圍

<h1>Recursion</h1>
Start with (0,0) (you can start with any point)
There must be a way to exit.
Use recursive to try any four direction, then use an array to save what directions it goes.
If the way is return true, keep returning true until the start of the recursion.
If the way is failed. pop out the latest array you push in.
The next step can't be neither start point nor out of range.

```javascript=
let findRoad = function (maze) {
    let road = [];
    helper(maze, road, 0, 0);
    return road;
}

let helper = function (maze, road, i, j) {
    if (i == maze.length - 1 && j == maze[i].length - 1) return true;

    /**
     * i+1 S
     * i-1 N
     * j+1 E
     *j-1 W
     */

    if (j < maze[i].length - 1 && maze[i][j + 1] !== 0) {
        maze[i][j + 1] = 0;
        road.push("E");
        if (helper(maze, road, i, j + 1) == true) return true;
    }
    if (i < maze.length - 1 && maze[i + 1][j] !== 0) {
        maze[i + 1][j] = 0;
        road.push("S");
        if (helper(maze, road, i + 1, j) == true) return true;
    }
    if (i > 0 && maze[i - 1][j] !== 0 && !(i == 1 && j == 0)) {
        maze[i - 1][j] = 0;
        road.push("N");
        if (helper(maze, road, i - 1, j) == true) return true;
    }
    if (j > 0 && maze[i][j - 1] !== 0 && !(i == 0 && j == 1)) {
        maze[i][j - 1] = 0;
        road.push("W");
        if (helper(maze, road, i, j - 1) == true) return true;
    }
    road.pop();
}

let maze = [
    [1, 1, 1, 1, 1],
    [0, 1, 0, 0, 1],
    [0, 1, 0, 0, 1],
    [0, 0, 1, 0, 1],
    [0, 0, 1, 1, 1],
]
let maze2 = [
    [1, 1, 1, 0, 1],
    [0, 1, 0, 0, 1],
    [0, 1, 1, 0, 1],
    [0, 0, 1, 0, 1],
    [0, 0, 1, 1, 1],
];
let maze3 = [
    [1, 1, 1, 1, 1],
    [1, 0, 0, 0, 0],
    [1, 0, 1, 1, 1],
    [1, 0, 1, 0, 1],
    [1, 1, 1, 0, 1],
];
let maze4 = [
    [1, 0, 0, 0, 1],
    [1, 1, 1, 0, 0],
    [1, 0, 1, 0, 1],
    [1, 0, 1, 0, 1],
    [1, 0, 1, 1, 1],
];
console.log(findRoad(maze));
console.log(findRoad(maze2));
console.log(findRoad(maze3));
console.log(findRoad(maze4));
```

<h1>迴圈</h1>
使用array紀錄走過的路，然後用String紀錄方向
起點先設為0(代表走過了)
判斷四個方向中不是0的路，走過去時用array紀錄「走之前的座標」，並用String紀錄剛剛的方向，然後把「現在要走的座標」設為0代表走過了。
當走到終點時把迴圈break掉
如果走到死路了，因為有用array紀錄剛剛的路，所以可以回到上一個點，String也要把剛剛最後紀錄的方向刪掉。
最後印出String就是答案了

<h1>Iteration </h1>
Use an array to record the road it just went, then use a string to record the directions.
The start point is set to 0.
Going four diections which is not 0, use array to record the position before go, then use string to record the direction. Remeber to set the point to 0, means it has passed this way.
To break the loop if it arrive the end.
If there's no way to go, use the latest item in array, then delete the latest char in String.

Print out the String, which is the answer.

```javascript=
let findRoad = function (maze) {
    return helper(maze, 0, 0);
}

let helper = function (maze, i, j) {
    let road = [];
    let roadStr = "";
    maze[0][0] = 0;
    while (true) {
        if(i === maze.length - 1 && j  === maze[i].length - 1) break;
        if (i < maze.length - 1 && maze[i + 1][j] !== 0) {
            road.push([i, j]);
            roadStr += "S";
            maze[i + 1][j] = 0;
            i++;
        } else if (i > 0 && maze[i - 1][j] !== 0) {
            road.push([i, j]);
            roadStr += "N";
            maze[i - 1][j] = 0;
            i--;
        } else if (j < maze[i].length - 1 && maze[i][j + 1] !== 0) {
            road.push([i, j]);
            roadStr += "E";
            maze[i][j + 1] = 0;
            j++;
        } else if (j > 0 && maze[i][j - 1] !== 0) {
            road.push([i, j]);
            roadStr += "W";
            maze[i][j - 1] = 0;
            j--;
        } else {
            let tmp = road.pop();
            i = tmp[0];
            j = tmp[1];
            roadStr = roadStr.substring(0, roadStr.length - 1);
        }
    }
    return roadStr;
}

let maze = [
    [1, 1, 1, 1, 1],
    [0, 1, 0, 0, 1],
    [0, 1, 0, 0, 1],
    [0, 0, 1, 0, 1],
    [0, 0, 1, 1, 1],
]
let maze2 = [
    [1, 1, 1, 0, 1],
    [0, 1, 0, 0, 1],
    [0, 1, 1, 0, 1],
    [0, 0, 1, 0, 1],
    [0, 0, 1, 1, 1],
];
let maze3 = [
    [1, 1, 1, 1, 1],
    [1, 0, 0, 0, 0],
    [1, 0, 1, 1, 1],
    [1, 0, 1, 0, 1],
    [1, 1, 1, 0, 1],
];
let maze4 = [
    [1, 0, 0, 0, 1],
    [1, 1, 1, 0, 0],
    [1, 0, 1, 0, 1],
    [1, 0, 1, 0, 1],
    [1, 0, 1, 1, 1],
];
let maze5 = [
    [1, 0, 1, 1, 1],
    [1, 1, 1, 0, 1],
    [0, 0, 1, 0, 1],
    [0, 1, 1, 0, 1],
    [1, 1, 0, 0, 1],
];
console.log(findRoad(maze));
console.log(findRoad(maze2));
console.log(findRoad(maze3));
console.log(findRoad(maze4));
console.log(findRoad(maze5));
```