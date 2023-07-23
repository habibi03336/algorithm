---
title: LeetCode, Merge two sorted list

date: 2022-10-10

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

val의 작은 순으로 연결된 singly linked list 2개가 주어진다. 2개의 sorted singly linked list를 합쳐 하나의 sorted singly list로 만들어 반환하라.

# 문제접근

1. 끼워넣는 개념으로, list를 직선적으로 조회하면서 sorted merged list를 만들 수 있다.(O(N))

2. 한 리스트를 끝까지 조회했다면 다른 리스트의 남은 부분은 merged list의 뒤에 붙여주면 그만이다.

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
 * @param {ListNode} list1
 * @param {ListNode} list2
 * @return {ListNode}
 */
var mergeTwoLists = function (list1, list2) {
	let head = new ListNode();
	let tail = head;

	const attachNode = (newNode) => {
		tail.next = newNode;
		tail = newNode;
	};

	while (true) {
		if (list1 === null || list2 === null) {
			attachNode(list1 === null ? list2 : list1);
			break;
		}

		const newNode = new ListNode();
		if (list1.val <= list2.val) {
			newNode.val = list1.val;
			list1 = list1.next;
		} else {
			newNode.val = list2.val;
			list2 = list2.next;
		}

		attachNode(newNode);
	}

	return head.next;
};
```
