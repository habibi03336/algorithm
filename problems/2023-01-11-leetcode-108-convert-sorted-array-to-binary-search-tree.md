---
title: LeetCode, 108. Convert Sorted Array to Binary Search Tree

date: 2023-1-11

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)

오름차순으로 정렬된 정수 배열이 주어진다. 배열을 height balanced BST(Binary Search Tree)로 변환하여 트리의 루트를 반환하라.

# 문제접근

1. height balanced된 BST를 만들기 위해서는 배열에서 중간에 있는 값이 부모가 되어야 한다. dfs와 재귀함수를 활용한다. 중간에 있는 값으로 루트 노드를 만들고, 재귀적으로 서브 트리를 만든다. 서브 트리에서도 재귀적으로 나뉜 배열의 중간 값을 루트 노드로 만든다.

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
 * @param {number[]} nums
 * @return {TreeNode}
 */
var sortedArrayToBST = function (nums) {
	const dfs = (start, end) => {
		if (start > end) return null;
		const mid = ((start + end) / 2) >> 0;
		const node = new TreeNode(nums[mid]);
		node.left = dfs(start, mid - 1);
		node.right = dfs(mid + 1, end);
		return node;
	};
	return dfs(0, nums.length - 1);
};
```
