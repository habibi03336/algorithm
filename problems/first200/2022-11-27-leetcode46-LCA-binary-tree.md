---
title: LeetCode, Lowest Common Ancestor of a Binary Tree

date: 2022-11-27
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

binary tree와 두 노드가 주어진다. 두 노드의 LCA를 찾아라. LCA는 Least Common Ancestor의 줄임말이다. 두 노드가 공통으로 가지는 조상 중 가장 가까운 조상을 찾으면 된다. 경우에 따라 주어진 노드도 LCA가 될 수 있다.

- 노드의 개수: \[2, 10^5\]
- 모든 노드의 val은 unique하다.
- 주어진 노드는 반드시 존재한다.
- 주어진 두 노드는 다른 노드이다.

# 문제접근

1. 루트 부터 주어진 노드까지의 경로(trace)를 찾으면 문제를 풀 수 있다. 주어진 두 노드까지 각각의 경로를 구한 후, 가장 가까운 공통 조상을 찾으면 된다.
	- 관련문제, Binary Search Tree의 LCA 찾기: `2022-10-14-leetcode10-LCA`

2. 경로를 찾기 위해서는 재귀함수를 이용해주면 된다. 재귀함수를 사용하면 경로를 트래킹해 줄 수 있다. 재귀함수가 복잡한 컨텍스트를 관리해주기 떄문이다.

# 코드

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function (root, p, q) {
	const trace = [];
	let pTrace;
	let qTrace;
	//dfs
	const tracingDfs = (node) => {
		if (node === null) return;
		trace.push(node);
		if (node.val === p.val) pTrace = [...trace];
		if (node.val === q.val) qTrace = [...trace];
		tracingDfs(node.left);
		tracingDfs(node.right);
		trace.pop();
	};
	tracingDfs(root);
	let i = 0;
	while (true) {
		if (pTrace[i] !== qTrace[i]) return pTrace[i - 1];
		i++;
	}
};
```
