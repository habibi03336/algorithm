---
title: LeetCode, Unique Paths

date: 2022-12-17
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/unique-paths/description/)

크기가 m x n 인 grid가 주어진다. 로봇은 0,0 포지션에 있고, 목표지점은 m-1, n-1 이다. 로봇은 오른쪽 혹은 아래쪽으로 움직일 수 있다. 로봇이 목표지점까지 이동하는 unique 경로의 개수를 반환하라.

## 문제접근

1. x, y 포지션까지 가는 경로의 개수는 (x, y - 1 포지션까지 경로의 개수) + (x - 1, y 포지션까지 경로의 개수)이다. 점화식이 명확하게 나오니, DP 방식으로 풀 수 있을 것이다. 각 위치에 대해서 한 번씩 계산을 하므로 O(n\*m)

- DP(동적계획법) : 문제를 여러 개의 하위 문제로 나눈 후, 하위 문제를 먼저 계산한 후 그 값을 저장하여 사용하는 방식으로 최종 해를 구하는 방식

## 구현 방식

1. grid를 나타내는 board를 이차원 배열로 만들고, i, j 위치의 해를 (i-1, j 위치의 해) + (i, j-1 위치의 해)로 구한 후, 목표지점의 값을 반환한다.

2. 공간복잡도에 대해서 최적화를 할 수 있다. (i-1 , j 위치의 해)를 기존 배열의 값으로, (i, j-1 위치의 해를) 이전 요소의 값으로 나타낼 수 있다. 따라서 필요한 하위 문제의 저장값을 하나의 배열로만 관리할 수 있다.

## 코드

### 공간복잡도 최적화 X

54 ms, 42.3 MB

```javascript
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var uniquePaths = function (m, n) {
	const board = Array(m)
		.fill()
		.map(() => Array(n).fill(1));
	for (let i = 1; i < m; i += 1) {
		for (let j = 1; j < n; j += 1) {
			board[i][j] = board[i][j - 1] + board[i - 1][j];
		}
	}
	return board[m - 1][n - 1];
};
```

### 공간복잡도 최적화 O

54 ms, 41.7 MB

```javascript
var uniquePaths = function (m, n) {
	const board = Array(n).fill(1);
	for (let i = 1; i < m; i += 1) {
		for (let j = 1; j < n; j += 1) {
			board[j] = board[j] + board[j - 1];
		}
	}
	return board[n - 1];
};
```
