---
title: LeetCode, 92. Reverse Linked List II

date: 2023-1-18

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/reverse-linked-list-ii/description/)

길이가 n인 연결 리스트의 해드가 주어진다. 또 연결리스트의 일부분의 시작하는 지점을 의미하는 left, 끝나는 노드를 의미하는 right가 주어진다. 순서대로 left부터 right까지의 연결리스트 부분만 뒤집어라.

- n의 범위: [1, 500]

# 문제접근

1. 연결리스트를 세 부분으로 나눌 수 있다. 뒤짚는 부분, 뒤짚는 부분의 앞, 뒤짚는 부분의 뒤.

1. `뒤짚어야 하는 부분의 뒤짚은 후` head와 tail, `앞 쪽 부분`의 tail, `뒤 쪽 부분`의 head가 필요하다.

# 구현

1. 맨 처음 노드가 뒤짚는 부분에 포함되냐 안되냐에 따라서 처리가 달라진다. 맨 처음 노드가 뒤짚는 부분에 포함되면, `앞 쪽 부분`의 tail이 null이 된다. 또한 뒤짚은 후 head가 전체에서도 head가 되어야한다. 관련된 예외처리가 많아서 헷갈렸다.

2. 처음에 dummy를 하나 넣어주므로써 데이터 타입을 맞춰주고, 예외처리를 줄일 수 있다.

# 코드

## dummy 사용

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
 * @param {number} left
 * @param {number} right
 * @return {ListNode}
 */
var reverseBetween = function (head, left, right) {
	const dummy = new ListNode(Number.Infinity, head);
	let i = 1;
	let node = dummy;
	while (i < left) {
		node = node.next;
		i++;
	}
	const preTail = node;
	let prev = node;
	node = node.next;
	const midTail = node;
	while (i <= right) {
		const tmp = node.next;
		node.next = prev;
		prev = node;
		node = tmp;
		i++;
	}
	midTail.next = node;
	preTail.next = prev;
	return dummy.next;
};
```

## dummy 사용 안함

```javascript
var reverseBetween = function (head, left, right) {
	if (head.next === null) return head;
	let i = 1;
	let node = null;
	while (i < left) {
		node = node === null ? head : node.next;
		i++;
	}
	const preHead = node;
	let prev = node;
	node = node === null ? head : node.next;
	const midHead = node;
	while (i <= right) {
		const tmp = node.next;
		node.next = prev;
		prev = node;
		node = tmp;
		i++;
	}
	midHead.next = node;
	if (preHead === null) head = prev;
	else preHead.next = prev;
	return head;
};
```
