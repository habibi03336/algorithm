---
title: LeetCode, Binary Tree Level Order Traversal

date: 2022-11-2
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/binary-tree-level-order-traversal/)

트리의 루트 노드가 주어졌을 때, 트리의 전체의 노드 값을 층별로 배열에 담아 배열로 반환해라.

> EX)  
> Input: root = [3,9,20,null,null,15,7]  
> Output: [[3],[9,20],[15,7]]

- 노드의 개수는 0 ~ 2000
- 노드의 값은 -1000 ~ 1000

# 문제접근

1. BFS를 deque을 활용해 구현하면 된다. 층 별 단위로 끊어줘야 한다. while문 안에서 deque의 length 만큼만 pop 하도록해서 층 별 구분을 해줄 수 있다. 노드를 한 번씩만 조회하므로 O(N)이다.

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
 * @return {number[][]}
 */
var levelOrder = function (root) {
	if (root === null) return [];
	const deque = [root];
	const result = [];
	while (deque.length !== 0) {
		const deqLen = deque.length;
		const levelNodes = [];
		for (let i = 0; i < deqLen; i++) {
			const node = deque.shift();
			levelNodes.push(node.val);
			if (node.left !== null) deque.push(node.left);
			if (node.right !== null) deque.push(node.right);
		}
		result.push(levelNodes);
	}
	return result;
};
```
