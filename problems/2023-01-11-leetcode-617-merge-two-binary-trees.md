---
title: LeetCode, 617. Merge Two Binary Trees

date: 2023-1-11

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/merge-two-binary-trees/description/)

두 이진트리의 루트가 주어진다. 두 이진트리를 합친다. 단 이진트리의 같은 위치에 있는 노드끼리의 값은 더한다. 이러한 방식으로 두 노드를 합쳐 반환하라.

# 문제접근

1.

1. 각 노드에 대해서 만약 겹치는 노드가 있으면, 두 값을 합쳐 새로운 노드를 만든다.
2. 한 쪽 트리에만 존재하는 노드이면 해당 노드를 바로 반환한다.
3. 양 쪽다 존재하지 않으면 null을 반환한다.

# 구현

1. 재귀를 활용하여 dfs를 하면, 각 노드 및 서브트리에 대해서 같은 로직을 간단히 적용 할 수 있다.

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
 * @param {TreeNode} root1
 * @param {TreeNode} root2
 * @return {TreeNode}
 */
var mergeTrees = function (root1, root2) {
	if (root1 && root2) {
		const node = new TreeNode(root1.val + root2.val);
		node.left = mergeTrees(root1.left, root2.left);
		node.right = mergeTrees(root1.right, root2.right);
		return node;
	} else {
		return root1 || root2 || null;
	}
};
```
