---
title: LeetCode, 77. Combinations

date: 2023-1-8
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/combinations/description/)

두 자연수 n과 k가 주어젔을 때, 1에서 n까지의 자연수로 만들 수 있는 길이가 k인 모든 조합을 반환하라.

- 1 <= n <= 20
- 1 <= k <= n

# 문제접근

1. 그래프 경로 탐색 문제로 볼 수 있다. dfs와 backtrack 기법을 활용하여 풀이할 수 있다.

2. 조합의 경우 순열과는 다르게 (1,2) (2,1)이 같은 것이기 때문에 오름차순으로 정렬된 케이스만 반환하면 된다. 따라서 선택된 노드보다 큰 값을 가지는 노드로만 경로가 있는 것으로 생각해주면 된다.

# 코드

```javascript
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
var combine = function (n, k) {
	const res = [];
	const combi = [];
	const dfs = (start) => {
		if (combi.length === k) {
			res.push(combi.slice());
			return;
		}
		for (let i = start; i < n + 1; i++) {
			combi.push(i);
			dfs(i + 1);
			combi.pop();
		}
	};
	dfs(1);
	return res;
};
```
