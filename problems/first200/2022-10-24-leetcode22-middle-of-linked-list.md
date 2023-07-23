---
title: LeetCode, Middle of Linked List

date: 2022-10-24
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/middle-of-the-linked-list/)

singly linked list의 head가 주어졌을 때 linked list의 중간 노드를 반환하라.

- 중간 노드가 두 개일 경우 뒤에 것을 반환하라.
- 노드의 개수는 1 ~ 100

# 문제접근

1. array에 node를 차곡차곡 넣고, array length의 절반인 index에 접근하면 중간 노드에 접근할 수 있다.

# 코드

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var middleNode = function (head) {
	const array = [];
	let node = head;
	while (node !== null) {
		array.push(node);
		node = node.next;
	}

	return array[Math.floor(array.length / 2)];
};
```
