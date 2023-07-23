---
title: LeetCode, 234. Palindrome Linked List

date: 2023-1-17

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/palindrome-linked-list/description/)

길이가 n인 연결 리스트의 해드가 주어진다. 연결 리스트가 팔린드롬이면 true 아니면 false를 반환하라.

- n의 범위: [1, 100,000]

# 문제접근

1. 리스트에 값을 저장하여, 리스트를 조회하여 팔린드롬인지 판단해 줄 수 있다. 공간복잡도가 O(N)이다.

2. 슬로우, 패스트 **러너 기법**을 활용하여서 중간에 있는 노드를 찾는 방식을 응용할 수 있다. 슬로우, 패스트 러너를 달리게 하고 슬로우 러너는 연결리스트를 뒤집으면서 가도록한다. 중간 노드를 찾으면 뒤집힌 앞 쪽 연결리스트와, 뒤집히지 않은 뒤 쪽 연결리스트를 하나씩 비교해가면서 팔린드롬인지 확인해 줄 수 있다. 공간복잡도가 O(1)이다.

# 코드

## 리스트 이용

100 ms, 56 MB

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
 * @return {boolean}
 */
var isPalindrome = function (head) {
	const li = [];
	let node = head;
	while (node !== null) {
		li.push(node.val);
		node = node.next;
	}
	for (let i = 0; i < li.length / 2; i++) {
		if (li[i] !== li[li.length - 1 - i]) return false;
	}
	return true;
};
```

## 러너 기법 활용

181 ms, 72.5 MB

```javascript
var isPalindrome = function (head) {
	let prev = null;
	let slow = head;
	let fast = head;
	while (fast !== null && fast.next !== null) {
		fast = fast.next.next;
		const tmp = slow.next;
		slow.next = prev;
		prev = slow;
		slow = tmp;
	}
	if (fast !== null) {
		slow = slow.next;
	}
	while (slow !== null) {
		if (prev.val !== slow.val) return false;
		prev = prev.next;
		slow = slow.next;
	}
	return true;
};
```
