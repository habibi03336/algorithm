---
title: LeetCode, 240. Search a 2D Matrix II

date: 2023-1-24

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/search-a-2d-matrix-ii/description/)

행 기준, 열 기준 모두 오름차순으로 정렬되어 있는 2차원 배열 matrix가 주어진다. 또 찾아야하는 값 target이 주어진다.

- m == matrix.length
- n == matrix[i].length
- 1 <= n, m <= 300

# 문제접근

1. dfs 개념으로 풀어줄 수 있다. (0,0)에서 시작하고, matrix 요소가 target보다 작은 경우에만 오른쪽과 아래쪽 요소를 탐색하도록 해주면 효율적으로 탐색할 수 있다.

2. 첫 번째 행 마지막 열에서 시작한다. 그리고 해당 위치의 요소가 target보다 작은 경우는 다음 행으로 가고, target보다 큰 경우에는 왼쪽 열로 이동해주는 방식으로 찾아줄 수 있다.

- 마지막 행, 첫 번째 열에서 시작해도 된다. target보다 작은 경우 다음 열로 이동하고, target보다 큰 경우는 위쪽 행으로 이동해주면 된다.

# 코드

## dfs 풀이

```javascript
/**
 * @param {number[][]} matrix
 * @param {number} target
 * @return {boolean}
 */
var searchMatrix = function (matrix, target) {
	let res = false;
	const dfs = (i, j) => {
		if (
			i < 0 ||
			j < 0 ||
			i === matrix.length ||
			j === matrix[0].length ||
			matrix[i][j] === null
		) {
			return;
		}
		if (matrix[i][j] === target) {
			res = true;
			return;
		}
		if (matrix[i][j] < target) {
			dfs(i + 1, j);
			dfs(i, j + 1);
		}
		matrix[i][j] = null;
	};
	dfs(0, 0);
	return res;
};
```

## 규칙 활용 풀이

```javascript
var searchMatrix = function (matrix, target) {
	let [i, j] = [0, matrix[0].length - 1];
	while (i < matrix.length && j >= 0) {
		if (matrix[i][j] > target) {
			j -= 1;
		} else if (matrix[i][j] < target) {
			i += 1;
		} else if (matrix[i][j] === target) {
			return true;
		}
	}
	return false;
};
```
