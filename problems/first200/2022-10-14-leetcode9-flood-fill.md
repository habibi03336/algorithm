---
title: LeetCode, Flood Fill

date: 2022-10-14

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/flood-fill/)

2차원 array가 주어진다. 2차원 array에서 시작점이 되는 sr, sc가 주어진다. 시작점을 주어진 color 값으로 변경한다. 시작점에서 위,아래,오른쪽,왼쪽으로 이동할 수 있다. 다만 이동하기 위해서는 시작점의 원래 값과 같은 값을 가지고 있어야한다. 이동 후 그 지점의 값을 주어진 color 깂으로 변경한다. 이러한 작업을 반복한다.

# 문제접근

1. 2차원 array를 traverse하면서 원하는 작동을 하는 문제이다. 이미 처리된 지점을 다시 방문할 경우 무한루프가 돌기 때문에 각 지점의 방문 여부를 체크해주는 2차원 배열이 하나 필요하다.

2. 방문할 지점을 저장하기 위해서 FIFO 개념으로 deque를 썼다.(stack을 써도 된다.)  
1)새로운 지점이 valid하고  
2)조건을 만족하며(source 값과 같은 값을 가짐)  
3)이미 방문된 지점이 아니면  
=> 방문할 지점으로 추가하고, 방문되었음(될 것임)을 표시한다.

# 코드

```javascript
/**
 * @param {number[][]} image
 * @param {number} sr
 * @param {number} sc
 * @param {number} color
 * @return {number[][]}
 */
var floodFill = function (image, sr, sc, color) {
	const visited = Array(image.length)
		.fill()
		.map(() => Array(image[0].length).fill(false));

	const sourceColor = image[sr][sc];
	const directions = [
		[-1, 0],
		[1, 0],
		[0, 1],
		[0, -1],
	];

	const deque = [[sr, sc]];
	while (deque.length !== 0) {
		const [x, y] = deque.shift();
		image[x][y] = color;

		directions.forEach((elem) => {
			const newX = elem[0] + x;
			const newY = elem[1] + y;
			if (
				newX >= 0 &&
				newX < image.length &&
				newY >= 0 &&
				newY < image[0].length &&
				image[newX][newY] === sourceColor &&
				!visited[newX][newY]
			) {
				deque.push([newX, newY]);
				visited[newX][newY] = true;
			}
		});
	}

	return image;
};
```
