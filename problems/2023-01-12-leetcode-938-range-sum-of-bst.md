---
title: LeetCode, 938. Range Sum of BST

date: 2023-1-12

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/range-sum-of-bst/)

이진 탐색 트리가 주어진다. 또 범위 low와 high가 주어진다. low 이상 high 이하인 노드의 값을 모두 합하여라.

- 노드의 값은 unique하다.

# 문제접근

1. 노드를 순회하면서 조건에 맞는 값을 모두 더하면 된다. 이 때, 노드 값이 low 보다 작으면 왼쪽 자식노드에는 low 이상 값이 없는 것이 확실하므로 오른쪽 자식만 순회를 해주고, 반대로 노드 값이 high 보다 크면 오른쪽 자식노드에는 high 이하 값이 없는 것이 확실하므로 왼쪽 자식만 순회를 하는 방식으로 가지치기를 해줄 수 있다.

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
 * @param {number} low
 * @param {number} high
 * @return {number}
 */
var rangeSumBST = function (root, low, high) {
	let res = 0;
	const dfs = (node) => {
		if (node === null) return;
		if (node.val < low) {
			dfs(node.right);
		}
		if (node.val > high) {
			dfs(node.left);
		}
		if (node.val >= low && node.val <= high) {
			res += node.val;
			dfs(node.left);
			dfs(node.right);
		}
	};
	dfs(root);
	return res;
};
```
