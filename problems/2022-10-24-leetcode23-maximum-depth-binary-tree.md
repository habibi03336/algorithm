---
title: LeetCode, Maximum Depth of Binary Tree

date: 2022-10-24
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

binary tree의 root가 주어졌을 때, tree의 maximum depth(height)를 구하여라.

- node는 0 ~ 10,000

# 문제접근

1. 각 depth 별로 구간을 나누어 breath first search를 하면서, 가장 깊은 depth까지 구해주면된다.

2. recursion을 활용하여 풀어줄 수도 있다.

# 코드

## breath first search

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
var maxDepth = function (root) {
	if (root === null) return 0;

	let deque = [root];
	let depth = 0;
	while (deque.length !== 0) {
		depth++;
		const dequeLen = deque.length;
		for (let i = 0; i < dequeLen; i++) {
			const node = deque.shift();
			if (node.left !== null) deque.push(node.left);
			if (node.right !== null) deque.push(node.right);
		}
	}

	return depth;
};
```

## recursion

```javascript
const maxDepth = (subRoot) => {
	if (subRoot === null) return 0;
	return Math.max(maxDepth(subRoot.right), maxDepth(subRoot.left)) + 1;
};
```
