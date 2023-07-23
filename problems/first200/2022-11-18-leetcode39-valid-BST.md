---
title: LeetCode, Valid BST

date: 2022-11-18
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/validate-binary-search-tree/submissions/)

이진탐색을 자료구조로 나타낸 것을 BST(Binary Search Tree)라고 한다. 주어진 이진트리가 유효한 이진탐색트리인지 확인하여라.

- 트리 노드의 개수는 [1, 10^4]
- 노드 값은 [-2^31, 2^31 - 1]

# 문제접근

1. 모든 노드를 traverse 하면서 특정 조건을 만족하는지 확인해줘야 한다.(O(N)) 이 때 노드에 따라 만족해야 하는 조건이 달라진다. 이런 경우 recursion을 활용하면 보다 간단하게 문제에 접근할 수 있다.recursion을 활용하면 해당 context에 따른 값을 동적으로 넣어주기 편하기 때문이다. 그렇지 않으면 노드에 따라서 조건을 일일이 바꾸어 줘야하는데 복잡도가 너무 높아 거의 불가능하다.
	- 관련 문제: `2022-11-05-leetcode34-course-schedule`
	
2. 처음에 착각 했던 것이 자신의 부모 노드보다만 작거나 크거나 하면 되는 것으로 생각했다. 이는 오해였다. BST가 되기 위해서는 ancestor를 모두 고려한 상하방 조건을 모두 만족해야한다.

# 코드

```javascript
var isValidBST = function (root) {
	let result = true;
	const recursiveSearch = (node, min, max) => {
		const value = node.val;
		if (value <= min || value >= max) result = false;
		if (node.left !== null) recursiveSearch(node.left, min, value);
		if (node.right !== null) recursiveSearch(node.right, value, max);
	};
	const MAX_VAL = 2 ** 31;
	const MIN_VAL = -(2 ** 31) - 1;
	recursiveSearch(root, MIN_VAL, MAX_VAL);

	return result;
};
```
