---
title: LeetCode, 783. Minimum Distance Between BST Nodes

date: 2023-1-12

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/minimum-distance-between-bst-nodes/description/)

이진 탐색 트리가 주어진다. 두 노드 값의 거리(차이의 절댓값) 중 가장 작은 거리를 반환하라.

- 노드의 개수: [2, 100]

# 문제접근

1. 정렬된 배열이라고 했을 때, 앞 뒤로만 비교하면서 가장 작은 거리를 찾아줄 수 있다. 이와 같은 논리로 노드를 왼쪽 자식부터 중위순회를 하면서 오름차순으로 노드의 값을 조회하면서 가장 가까운 거리를 찾아주면 된다.

# 구현

1. 처음에 앞 선 비교 대상이 없을 때는 무시해주기 위해서, n(가장 가까운 작은 값)을 - Number.MAX_SAFE_INTEGER로 초기화 한다.

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
 * @return {number}
 */
var minDiffInBST = function (root) {
	let res = Number.MAX_SAFE_INTEGER;
	let n = -Number.MAX_SAFE_INTEGER;
	const dfs = (node) => {
		if (node === null) return;
		dfs(node.left);
		res = Math.min(res, node.val - n);
		n = node.val;
		dfs(node.right);
	};
	dfs(root);
	return res;
};
```
