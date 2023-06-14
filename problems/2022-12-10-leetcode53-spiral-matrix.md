---
title: LeetCode, Spiral Matrix

date: 2022-12-10
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/spiral-matrix/)

m x n의 2차원 배열이 주어졌을 때, spiral order로 순회한 element의 배열을 반환하라.

- spiral order이란 회오리 모양으로 왼쪽 -> 오른쪽, 위 -> 아래, 오른쪽 -> 왼쪽, 아래 -> 위, .... 이렇게 반복하는 접근을 말한다. (문제 그림 참고)
- m, n의 범위는 [1, 10]
- 배열의 요소의 범위는: [-100, 100]

# 문제접근

1. 배열의 끝까지 가거나, 이미 방문된 배열의 요소를 만나면 방향을 바꾸게 된다.

2. 이차원 배열의 모든 요소를 순회했다면 결과를 반환하면 된다.

# 구현 방식

1. 방향을 배열로 정의(directions)해 놓고, 방향을 바꿀 때 참조하는 index(dir)를 갱신하는 식으로 했다. 모듈로 연산(%)을 통해서 보다 쉽게 반복되는 방향을 제어할 수 있다.

2. 마지막에 모든 요소를 탐색한 것을 어떻게 확인하고 제어할까 조금 고민했다. 두 가지 정도의 방법이 생각났는데 보다 직관적인 첫 번째 것을 선택했다.

1. 순회한 요소의 count(배열의 길이)가 전체 matrix 요소 개수와 같은지 확인하는 방법
2. 별도의 flag를 통해 연속으로 방향을 2번 바꾸는지 확인하는 방법(마지막 요소가 아닌 이상 새로운 요소를 방문하지 않고 두 번 연속 방향을 바꾸는 경우는 없음)

# 코드

```javascript
/**
 * @param {number[][]} matrix
 * @return {number[]}
 */
var spiralOrder = function (matrix) {
	let dir = 0;
	const directions = [
		[0, 1],
		[1, 0],
		[0, -1],
		[-1, 0],
	];
	const visited = Array(matrix.length)
		.fill()
		.map(() => Array(matrix[0].length).fill(false));
	const elements = [];
	const elementCount = matrix.length * matrix[0].length;
	let [i, j] = [0, 0];
	while (true) {
		if (!visited[i][j]) {
			visited[i][j] = true;
			elements.push(matrix[i][j]);
		}
		const [dx, dy] = directions[dir];
		if (
			j + dy < 0 ||
			i + dx >= matrix.length ||
			j + dy >= matrix[0].length ||
			visited[i + dx][j + dy]
		) {
			dir += 1;
			dir %= 4;
		} else {
			i += dx;
			j += dy;
		}
		if (elements.length === elementCount) break;
	}
	return elements;
};
```
