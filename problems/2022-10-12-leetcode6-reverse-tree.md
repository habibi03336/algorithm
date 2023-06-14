---
title: LeetCode, Reverse Tree

date: 2022-10-12

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

binary tree의 root가 주어진다. 주어진 binary tree를 거꾸로 바꾸어서(각 노드의 왼쪽 자식과 오른쪽 자식을 바꿔서) root를 반환하라.

- binary tree의 노드 개수는 N개이다.

# 문제접근

1. deque or stack을 사용하여 모든 노드를 방문하며 왼편 자식과 오른편 자식을 바꿔주면 된다. (breath first인지 depth first인지는 중요하지 않다.) (O(N))

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
var invertTree = function (root) {
	if (root === null) return null;

	const deque = [root];
	while (deque.length !== 0) {
		const node = deque.shift();
		const tmp = node.left;
		node.left = node.right;
		node.right = tmp;

		if (node.left !== null) deque.push(node.left);
		if (node.right !== null) deque.push(node.right);
	}

	return root;
};
```
