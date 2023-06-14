---
title: LeetCode, Number of Islands

date: 2022-11-19
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/number-of-islands/)

m \* n의 이차원 array가 주어진다. 값이 '1'이면 섬 값이 '0'이면 물이다. 상하좌우로 '1'이 붙어있으면 하나의 섬으로 본다. 섬의 개수를 반환하라.

- array 외부는 모두 물이라고 가정한다.
- m과 n의 범위는 [1, 300] 이다.

# 문제접근

1. 이차원 array를 탐색하는 문제이다. 각 위치를 대략 한 번씩 탐색하므로 O(N)의 시간복잡도를 기대할 수 있다.

2. recursion을 이용해서 하나의 섬을 탐색해줄 수도 있고, deque를 활용하여 탐색해줄 수도 있다.

# 코드

## recusion을 활용한 풀이

148 ms 44.5 MB

```javascript
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function (grid) {
	const counted = Array(grid.length)
		.fill()
		.map(() => Array(grid[0].length).fill(false));
	const marker = (x, y) => {
		if (x < 0 || y < 0 || x >= grid.length || y >= grid[0].length) return;
		if (grid[x][y] === "1" && !counted[x][y]) {
			counted[x][y] = true;
			marker(x + 1, y);
			marker(x, y + 1);
			marker(x - 1, y);
			marker(x, y - 1);
		}
	};

	let islandCount = 0;
	for (let i = 0; i < grid.length; i++) {
		for (let j = 0; j < grid[0].length; j++) {
			if (grid[i][j] === "1" && !counted[i][j]) {
				islandCount += 1;
			}
			marker(i, j);
		}
	}

	return islandCount;
};
```

## deque을 활용한 풀이

195 ms 51.7 MB

```javascript
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function (grid) {
	const counted = Array(grid.length)
		.fill()
		.map(() => Array(grid[0].length).fill(false));
	const directions = [
		[1, 0],
		[0, 1],
		[-1, 0],
		[0, -1],
	];
	const marker = (x, y) => {
		counted[x][y] = true;
		const deque = [[x, y]];
		while (deque.length !== 0) {
			const [x, y] = deque.shift();
			directions.forEach((dir) => {
				const nextX = x + dir[0];
				const nextY = y + dir[1];
				if (
					nextX < 0 ||
					nextY < 0 ||
					nextX >= grid.length ||
					nextY >= grid[0].length
				)
					return;
				if (grid[nextX][nextY] === "1" && !counted[nextX][nextY]) {
					deque.push([nextX, nextY]);
					counted[nextX][nextY] = true;
				}
			});
		}
	};

	let islandCount = 0;
	for (let i = 0; i < grid.length; i++) {
		for (let j = 0; j < grid[0].length; j++) {
			if (grid[i][j] === "1" && !counted[i][j]) {
				islandCount += 1;
				marker(i, j);
			}
		}
	}

	return islandCount;
};
```

recursion 풀이가 더 깔끔하고 속도도 더 괜찮게 나왔다.
