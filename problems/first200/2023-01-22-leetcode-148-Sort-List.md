---
title: LeetCode, 148. Sort List

date: 2023-1-22

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/sort-list/description/)

정수 값을 가지는 두 연결 리스트가 주어진다. 두 리스트를 값의 순서에 따라 오름차순으로 정렬하여 리스트의 헤드를 반환하라.

- O(1)의 공간복잡도와 O(nlogn)의 시간복잡도로 구현하라.

# 문제접근

1. 러너 기법을 사용하여서 중간 노드를 찾아줄 수 있고, 이를 활용하여 병합 정렬을 할 수 있다.

# 구현

1. 정렬된 부분을 재귀를 활용하여 합쳐줄 수 있다.

2. 정렬된 부분을 반복문을 활용하여 합쳐줄 수 있다.

- 재귀함수가 재귀함수를 내부적으로 한 번만 호출 하는 경우에는 반복문으로 쉽게 바꿔줄 수 있다.
- 재귀함수가 코드적으로는 보다 우아하고, 성능적으로는 반복문이 다소 우세하다.

# 코드

## 재귀함수 활용

210 ms, 70.5 MB

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
const mergeList = (list1, list2) => {
	if (list1 === null || (list2 !== null && list1.val > list2.val)) {
		[list1, list2] = [list2, list1];
	}
	if (list1 === null && list2 === null) return null;
	list1.next = mergeList(list1.next, list2);
	return list1;
};

var sortList = function (head) {
	if (head === null) return null;
	if (head.next === null) return head;
	let slow = head;
	let fast = head;
	let prev = head;
	while (fast !== null && fast.next !== null) {
		prev = slow;
		slow = slow.next;
		fast = fast.next.next;
	}
	prev.next = null;
	const left = sortList(head);
	const right = sortList(slow);
	return mergeList(left, right);
};
```

## 반복문을 활용

174 ms, 67.2 MB

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
var sortList = function (head) {
	if (head === null) return null;
	if (head.next === null) return head;
	let slow = head;
	let fast = head;
	let prev = head;
	while (fast !== null && fast.next !== null) {
		prev = slow;
		slow = slow.next;
		fast = fast.next.next;
	}
	prev.next = null;
	let left = sortList(head);
	let right = sortList(slow);

	const root = new ListNode();
	let node = root;
	while (left !== null || right !== null) {
		if (left === null || (right !== null && left.val > right.val)) {
			tmp = left;
			left = right;
			right = tmp;
		}
		node.next = left;
		node = node.next;
		left = left.next;
	}
	return root.next;
};
```
