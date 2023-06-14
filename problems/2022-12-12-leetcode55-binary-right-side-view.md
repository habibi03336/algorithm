---
title: LeetCode, Binary Right Side View

date: 2022-12-12
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/binary-tree-right-side-view/)

binary tree의 root가 주어진다. binary tree를 오른쪽에서 본다고 했을 때, 보이는 노드의 value를 위에 있는 것부터 배열에 담아 반환하라.

- tree의 node 개수의 범위: [0, 100]
- node value의 범위: [-100, 100]

# 문제접근

1. '오른쪽에서 보이는 노드'는 다른 말로, 각 이진트리의 깊이에서 가장 오른쪽에 있는 노드를 의미한다. BFS를 하면서 각 깊이에서 가장 오른쪽에 있는 노드를 찾아주면된다. (O(N))

# 구현 방식

1. BFS를 하면서 각 level 단위로 끊어서 가장 오른쪽에 있는 노드를 확인해줘야 하기 때문에 while문 안에서 for문과 dequeLen(그 level에 있는 노드 개수, 초기 deque.length)를 활용하여 각 깊이 별로 제어해준다.

2. deque에 각 level의 가장 왼쪽 노드부터 가장 오른쪽 노드까지 순서대로 쌓는다. 따라서 각 레벨에서, deque의 가장 오른쪽에 있는 노드가 rightViewNodes가 된다.

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
 * @return {number[]}
 */
var rightSideView = function (root) {
	if (root === null) return [];
	//BFS
	const deque = [root];
	const rightViewNodes = [];
	while (deque.length !== 0) {
		const dequeLen = deque.length;
		rightViewNodes.push(deque[dequeLen - 1].val);
		for (let i = 0; i < dequeLen; i++) {
			const node = deque.shift();
			if (node.left !== null) deque.push(node.left);
			if (node.right !== null) deque.push(node.right);
		}
	}
	return rightViewNodes;
};
```
