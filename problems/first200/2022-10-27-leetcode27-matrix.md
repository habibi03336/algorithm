---
title: LeetCode, Matrix

date: 2022-10-27
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/01-matrix/)

0과 1로 이루어진 2d array(matrix) mat이 주어진다. 각 지점에서 가장 가까운 0까지의 거리를 2d array로 반환하라.

- 위, 아래, 오른쪽, 왼쪽으로 움직이는 것이 한 단위의 거리이다.
- mat.length, mat\[0\].length는 1 ~ 10,000
- matrix의 크기는 1 ~ 10,000
- matrix에 0이 하나는 있다.

# 문제접근

1. 0이 있는 곳에서 시작해서 0 주변을 1단위로 접근할 수 있음을 표기, 이후 1단위 접근 주변을 2단위로 접근할 수 있음 표기, ... 이러한 방식을 반복하여 답을 구해줄 수 있다. 모든 칸에 대해서 같은 작업을 한 번 씩 수행하므로 O(N)의 시간 복잡도를 기대할 수 있다.

- 다음 단위로서 방문할 칸을 deque를 활용해 FIFO로 저장해준다. (breath first search의 개념)
- 이미 가까운 거리가 표기된 칸은 이미 더 적은 거리로 접근된 것이므로 update하지 않는다.(null인 경우만 표기 및 추후 방문한다.)

**⭐Tip**

1. mat\[x\]\[y\]라고 할 때, x는 row의 순번 y는 column의 순번을 나타낸다.

2. matrix에서 칸을 움직일 때 움직인 칸에 대해서 유효한 위치인지 혹은 조건에 만족하는지 확인해야 한다. 이 때 아래와 같이 움직이는 방향을 미리 array로 만들고 for문을 통해 코드를 작성하는 것이 깔끔하다.

3. deque를 통해 breath first search를 할 때, 이 경우처럼 단위로 끊어주어야 하는 경우가 있다. 미리 받아놓은 deque의 length와 for문을 통해서 딱 해당 단위 만큼만 shift를 하도록 할 수 있다.

# 코드

```javascript
/**
 * @param {number[][]} mat
 * @return {number[][]}
 */
var updateMatrix = function (mat) {
	const deque = [];
	for (let i = 0; i < mat.length; i++) {
		for (let j = 0; j < mat[0].length; j++) {
			if (mat[i][j] === 0) {
				deque.push([i, j]);
			} else {
				mat[i][j] = null;
			}
		}
	}

	const moveDir = [
		[1, 0],
		[-1, 0],
		[0, 1],
		[0, -1],
	];
	let distance = 0;
	while (deque.length !== 0) {
		const deqLen = deque.length;
		for (let i = 0; i < deqLen; i++) {
			const [x, y] = deque.shift();
			for (let dir of moveDir) {
				const [dx, dy] = dir;
				if (
					x + dx >= 0 &&
					x + dx < mat.length &&
					y + dy >= 0 &&
					y + dy < mat[0].length &&
					mat[x + dx][y + dy] === null
				) {
					mat[x + dx][y + dy] = distance + 1;
					deque.push([x + dx, y + dy]);
				}
			}
		}
		distance++;
	}

	return mat;
};
```
