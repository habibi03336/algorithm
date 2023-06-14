---
title: LeetCode, Diameter of Binary Tree

date: 2022-10-24
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/diameter-of-binary-tree/)

binary tree의 head가 주어진다. 두 노드의 경로에 있는 엣지의 개수를 diameter라고 할 때, 해당 트리의 가장 큰 diameter를 구하여라.

- 노드의 개수는 1 ~ 10,000

# 문제접근

1. 주어진 root를 지나는 가장 큰 diameter는 "왼쪽 sub tree의 max depth + 오른쪽 sub tree의 max depth"이다.

2. subtree에서 더 큰 diameter가 나오기 위한 필요조건은 (더 큰 depth를 가지는 subtree의 depth - 2) > (더 작은 depth를 가지는 subtree의 depth)이다. 만약 그렇지 않으면 주어진 root를 지나는 diameter가 항상 더 크다. 또한 이 조건을 만족했을 때, 더 큰 depth를 가지는 subtree에서만 더 큰 diameter가 나올 가능성이 있다.

- 주어진 root의 subtree에 대해서, 더 큰 depth를 n, 더 작은 depth를 t이라고 하자. 주어진 root를 지나는 최대 diameter는 n + t이다. 더 큰 depth를 가지는 subtree의 최대 diameter는 2n-2이다. "2n - 2 > n + t <==> n - 2 > t"이다.

- 더 작은 depth를 가지는 subtree의 최대 diameter는 2t - 2이다. n >= t 조건 하에서 2t-2는 n + t보다 클 수 없다. 따라서 더 작은 depth를 가지는 subtree에서는 주어진 root를 지나는 diameter보다 큰 diameter를 기대할 수 없다.

3. tree가 어느정도 balanced 되어있다면, 위 조건에 따라 max depth를 조회하면 O(N) + O(N/2) + O(N/4) ... 이므로 총 O(N)의 시간복잡도를 가진다. (balanced 되어있지 않다면 O(N\*\*2)이다.)

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
 * @return {number}
 */

const maxDepth = (subRoot) => {
	if (subRoot === null) return 0;
	return Math.max(maxDepth(subRoot.right), maxDepth(subRoot.left)) + 1;
};

var diameterOfBinaryTree = function (root) {
	const leftDepth = maxDepth(root.left);
	const rightDepth = maxDepth(root.right);
	const localDiameter = leftDepth + rightDepth;

	const biggerDepth =
		leftDepth > rightDepth ? [leftDepth, root.left] : [rightDepth, root.right];
	const smallerDepth =
		leftDepth > rightDepth ? [rightDepth, root.right] : [leftDepth, root.left];

	if (smallerDepth[0] < biggerDepth[0] - 2) {
		return Math.max(diameterOfBinaryTree(biggerDepth[1]), localDiameter);
	}

	return localDiameter;
};
```
