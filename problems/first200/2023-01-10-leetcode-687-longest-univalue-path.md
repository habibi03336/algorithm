---
title: LeetCode, 687. Longest Univalue Path

date: 2023-1-10

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/longest-univalue-path/description/)

이진 트리의 root가 주어진다. 같은 값을 가지는 노드끼리의 가장 긴 경로의 길이를 반환하라.

- 경로의 길이는 노드 사이의 간선의 개수이다.
- 트리 안의 노드의 개수: [0, 10,000]

# 문제접근

1. 트리를 탐색하는 문제이다. 재귀를 활용해 dfs를 하여 탐색해주면 된다. 각 노드에서 가장 긴 길이를 갱신하고, 해당 노드가 가지는 길이를 상태값을 반환한다. 자식의 상태값을 활용해 부모노드에서 가장 긴 길이를 갱신하는 작업을 재귀적으로 한다.

# 구현

1. 왼쪽 자식의 상태값, 오른쪽 노드의 상태값을 받는다. 자식노드와 값이 같으면 상태값이 유효하므로 유지하고, 다르면 상태값을 -1로 초기화 시킨다. 자식의 상태값을 이용하여 가장 긴 길이(longest)를 갱신하고, 자신의 상태값을 생성해 반환한다.

# 코드

```javascript
/**
 * @param {TreeNode} root
 * @return {number}
 */
var longestUnivaluePath = function (root) {
	if (root === null) return 0;
	let longest = 0;
	const dfs = (node) => {
		let [left, right] = [-1, -1];
		if (node.left !== null) left = dfs(node.left);
		if (node.right !== null) right = dfs(node.right);
		if (node.left !== null && node.left.val !== node.val) left = -1;
		if (node.right !== null && node.right.val !== node.val) right = -1;
		longest = Math.max(longest, right + left + 2);
		return Math.max(left, right) + 1;
	};
	dfs(root);
	return longest;
};
```
