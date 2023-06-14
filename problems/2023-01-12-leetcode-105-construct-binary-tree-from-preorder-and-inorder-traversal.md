---
title: LeetCode, 105. Construct Binary Tree from Preorder and Inorder Traversal

date: 2023-1-12

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)

트리를 전위순회(NLR)한 결과와 중위순위(LNR)한 결과가 배열로 주어진다. 이 정보를 활용하여 원래의 트리 구조를 만들어 루트를 반환하라.

- 노드의 값들은 unique하다.

# 문제접근

1. 큰 문제를 더 작은 문제로 쪼개서 해결하는 분할 정복 방식으로 해결할 수 있다.

1. preorder의 맨 앞에 있는 숫자는 root이다.
2. root를 알면 inorder를 활용해 root 왼쪽과 오른쪽을 나눌 수 있다.
3. 이를 활용해, 서브트리의 preorder과 inorder를 만들어 줄 수 있다.
4. 같은 로직을 dfs & backtraking을 활용하여 적용한다.

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
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function (preorder, inorder) {
	if (preorder.length === 0) return null;
	const rootVal = preorder.shift();
	const root = new TreeNode(rootVal);
	const rootValIdx = inorder.indexOf(rootVal);
	const [leftInorder, rightInorder] = [
		inorder.slice(0, rootValIdx),
		inorder.slice(rootValIdx + 1),
	];
	root.left = buildTree(preorder.slice(0, leftInorder.length), leftInorder);
	root.right = buildTree(preorder.slice(leftInorder.length), rightInorder);
	return root;
};
```
