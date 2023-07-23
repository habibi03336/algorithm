---
title: LeetCode, Minimum Height Trees

date: 2023-1-3
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/minimum-height-trees/submissions/870097896/)

tree는 모든 노드가 이어져있고, 두 노드가 하나의 경로로만 이어져있는(cycle이 없는) undirected graph이다.

트리 구조의 그래프가 0부터 n-1로 이름 붙여진 n개의 노드와 노드 간의 엣지를 나타내는 edges 배열로 주어진다. 한 노드를 루트로 선택하면 그 트리의 높이가 결정된다. 루트에 대한 모든 경우의 수 중에서 높이가 가장 낮은 트리를 MHT(minimum height trees)라고 한다.

모든 MHT의 루트를 반환하라.

- 반환 순서는 아무렇게나 해도된다.
- 노드 개수(n)의 범위: \[1,2\*10,000\]
- 엣지의 개수: n - 1

## 문제접근

1. [GeeksForGeeks](https://www.geeksforgeeks.org/roots-tree-gives-minimum-height/)를 참고하여 풀이하였다. 리프노드를 한 겹씩 제거해가면서 가장 중앙에 있는 노드를 찾아줄 수 있다. 가장 중앙에 있는 노드는 한 개일 수도 있고 두 개 일 수도 있다. (문제에서 모든 경우의 수를 반환하라고 했지만 최대 2개이다.)

2. 리프노드를 제거하면서 그것과 연결된 인접노드의 degree(연결된 노드 개수)를 하나씩 줄여준다. 이후 인접노드의 degree가 1이면 새로운 리프노드로 생각해준다.

## 구현

1. 그래프를 나타내기 위해서 인접리스트를 활용하였다.

2. que와 que의 길이 반복문을 활용해 단계별로 접근하도록 하였다.

3. degrees라는 degree를 저장하는 배열을 활용해 리프인지 확인해주었다.

## 코드

```javascript
/**
 * @param {number} n
 * @param {number[][]} edges
 * @return {number[]}
 */
var findMinHeightTrees = function (n, edges) {
	if (n === 1) return [0];
	const degrees = Array(n).fill(0);
	const adjs = {};
	edges.forEach((elem) => {
		degrees[elem[0]]++;
		degrees[elem[1]]++;
		if (adjs[elem[0]] === undefined) {
			adjs[elem[0]] = [elem[1]];
		} else {
			adjs[elem[0]].push(elem[1]);
		}
		if (adjs[elem[1]] === undefined) {
			adjs[elem[1]] = [elem[0]];
		} else {
			adjs[elem[1]].push(elem[0]);
		}
	});
	const que = [];
	degrees.forEach((val, idx) => {
		if (val === 1) {
			que.push(idx);
		}
	});
	let v = n;
	while (v > 2) {
		const queLen = que.length;
		v = v - queLen;
		for (let i = 0; i < queLen; i++) {
			const node = que.shift();
			for (let adj of adjs[node]) {
				degrees[adj]--;
				if (degrees[adj] === 1) {
					que.push(adj);
				}
			}
		}
	}
	return que;
};
```
