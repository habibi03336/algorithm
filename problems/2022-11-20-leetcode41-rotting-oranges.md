---
title: LeetCode, Rotting Oranges

date: 2022-11-20
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/rotting-oranges/)

m \* n의 이차원 array가 주어진다. 해당 위치에 싱싱한 오렌지가 있으면 1, 썩은 오렌지가 있으면 2 아무 것도 없으면 0의 값이 있다. 하루가 지날 때마다 썩은 오렌지 상하좌우에 있는 싱싱한 오렌지도 상한다고 한다. 주어진 이차원 array이에 있는 모든 오렌지가 상한 오렌지가 되는 최수 일수를 반환하라.

- 만약 모든 오렌지가 상하는 것이 불가능하다면 -1을 반환한다.
- m, n의 범위는 [1, 10]

# 문제접근

1. 이차원 array를 탐색하는 문제이다. 각 위치를 대략 한 번씩 탐색하므로 O(N)의 시간복잡도를 기대할 수 있다.

2. deque로 FIFO를 활용하여 풀어줄 수 있다. deque 구간(일자) 별로 구분하여 몇일이 걸리는지 구해줄 수 있다.

```javascript
// deque을 구간 별로 접근 하는 기본 form
const deque = [ ... ];
while (deque.length !== 0) {
  const deqLen = deque.length;
  for (let i = 0; i < deqLen; i++) {
    const [x, y] = deque.shift();
    // push new elems to deque by some condition;
  };
}

```

# 코드

```javascript
/**
 * @param {number[][]} grid
 * @return {number}
 */
var orangesRotting = function(grid) {
    const deque = [];
    let countFresh = 0;
    for(let i = 0; i < grid.length; i++){
        for(let j = 0; j < grid[0].length; j++){
            if(grid[i][j] === 2) { deque.push([i, j]) };
            if(grid[i][j] === 1) { countFresh++ }
        }
    }
    const adjacents = [[1, 0], [0, 1], [-1, 0], [0, -1]];
    let countDay = 0;
    while(countFresh !== 0 && deque.length !== 0){
        countDay++;
        const deqLen = deque.length;
        for(let i = 0; i < deqLen; i++){
            const [x, y] = deque.shift();
            adjacents.forEach((dir)=>{
                const newX = x + dir[0];
                const newY = y + dir[1];
                if(newX < 0 || newY < 0 || newX >= grid.length || newY >= grid[0].length) return;
                if(grid[newX][newY] === 1) {
                    grid[newX][newY] = 2;
                    deque.push([newX, newY]);
                    countFresh--;
                }
            })
        }
    }
    return countFresh === 0 ? countDay : -1;
```
