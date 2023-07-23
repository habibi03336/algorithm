---
title: LeetCode, 1038. Binary Search Tree to Greater Sum Tree

date: 2023-1-12

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/binary-search-tree-to-greater-sum-tree/description/)

이진 탐색 트리가 주어진다. 각 노드에서 자신보다 큰 노드의 값을 모두 더한 이진 트리를 반환하라.

- 노드의 값은 unique하다.

# 문제접근

1. 이진 탐색 트리가 주어졌으므로, 노드보다 오른쪽에 있는 모든 노드의 값을 더해주라는 의미이다. 오른쪽 자식 노드부터 방문하는 중위순회를 이용하여 가장 큰 값부터 조회할 수 있다. 가장 큰 값부터 차례대로 조회하면서 누적을 해주면 각 노드에 더해줄 값을 구해줄 수 있다.

# 코드

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var bstToGst = function (root) {
	let acc = 0;
	const dfs = (node) => {
		if (node === null) return;
		dfs(node.right);
		acc += node.val;
		node.val = acc;
		dfs(node.left);
	};
	dfs(root);
	return root;
};
```
