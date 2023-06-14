---
title: LeetCode, Reversed Linked List

date: 2022-10-23
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/reverse-linked-list/)

singly linked list가 주어졌을 때, 이를 반대로 뒤집어 반환하라.

- N은 0 ~ 50,000

# 문제접근

1. stack을 활용하여 각 노드를 LIFO로 조회하면 거꾸로된 순서로 접근할 수 있다.

2. 다른 사람의 풀이를 보니 stack을 사용하지 않고, 두 개의 포인터 활용하여 풀 수도 있다는 것을 확인했다. 현재 노드에 대한 포인터와 이전 노드에 대한 포인터를 활용하여 반복적으로 link를 뒤짚어 준다.

3. recursion을 활용한 풀이도 가능하다.

# 코드

## stack을 활용한 풀이

```javascript
var reverseList = function (head) {
	if (head === null) return null;

	const stack = [];
	let node = head;
	while (node !== null) {
		stack.push(node);
		node = node.next;
	}

	let curNode = stack.pop();
	const reversedHead = curNode;
	while (stack.length !== 0) {
		const nextNode = stack.pop();
		curNode.next = nextNode;
		curNode = nextNode;
	}
	curNode.next = null;

	return reversedHead;
};
```

## 두 개의 포인터를 활용한 풀이

```javascript
var reverseList = function (head) {
	if (head === null) return null;

	let prev = null;
	let node = head;
	while (node !== null) {
		const next = node.next;
		node.next = prev;

		prev = node;
		node = next;
	}

	return prev;
};
```

## recursion을 활용한 풀이

```javascript
var reverseList = function (head) {
	const recursiveReverse = (node, prev) => {
		if (node === null) return prev;

		const next = node.next;
		node.next = prev;
		return recursiveReverse(next, node);
	};

	return recursiveReverse(head, null);
};
```
