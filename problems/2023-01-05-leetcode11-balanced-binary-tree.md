---
title: LeetCode, Balanced Binary Tree

date: 2023-1-5
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/balanced-binary-tree/description/)

이진 트리의 node가 주어질 때, 해당 트리가 height-blanced 되어있는지 판단하라.

- height-blanced: 모든 노드에 대해서 왼쪽 subtree와 오른쪽 subtree의 height가 최대 1까지 밖에 차이나지 않는 상태.

* 노드의 개수의 범위: [0, 5000]

## 문제접근

1. [이 글을 참고하였다.](https://www.programiz.com/dsa/balanced-binary-tree)

2. 특정 노드에서 height-balanced되기 위해서는 아래 두 가지 조건을 만족해야한다.

1. 왼쪽 subtree와 오른쪽 subtree가 height balanced되어 있어야 한다.
2. 왼쪽 subtree의 height와 오른쪽 subtree의 height가 최대 1까지만 차이나야 한다.

## 구현

1. 트리를 탐색할 때는 재귀 구조를 활용하여 각 노드를 확인해 줄 수 있다. 왼쪽 subtree와 오른쪽 subtree에 대해서 각각 재귀함수를 적용하면 가장 왼쪽 아래에 있는 노드부터 조회하기 시작한다. (DFS)

- 재귀의 구조에 따라서 inorder, preorder, postorder 3가지 방식으로 조회할 수 있다.

- 이 문제에서는 postorder(후위순회)를 활용한다.

## 코드

```javascript
var isBalanced = function (root) {
	if (!root) return true;

	const isHeightBalanced = (root) => {
		if (root === null) return [true, 1];

		const [l, leftHeight] = isHeightBalanced(root.left);
		const [r, rightHeight] = isHeightBalanced(root.right);

		if (!(l && r)) return [false, 0];
		if (Math.abs(leftHeight - rightHeight) > 1) return [false, 0];
		return [true, Math.max(leftHeight, rightHeight) + 1];
	};

	return isHeightBalanced(root)[0];
};
```
