---
title: LeetCode, 328. Odd Even Linked List

date: 2023-1-18

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/odd-even-linked-list/description/)

길이가 n인 연결 리스트의 해드가 주어진다. 홀수 번째 노드가 앞 쪽에, 짝수 번째 노드는 뒷 쪽에 있도록 변경된 연결리스트를 반환하라. 단, 홀수 노드끼리, 짝수 노드 끼리의 순서는 지켜져야 한다.

- n의 범위: [1, 10,000]

# 문제접근

1. 홀수 번째 노드끼리 연결하고, 짝수 번째 노드끼리 연결한 후 둘을 이어붙이면 된다.

# 코드 정리 과정

1. 홀수 번째에 대한 로직과 짝수 번째에 대한 로직이 다므르로, while문 안에서 i를 통해 둘을 구분하도록 했다.

=> 홀수와 짝수에 대한 처리가 다르지만, while문 한 번에 홀수 단계와 짤수 단계를 한 번에 처리해 주는 것으로 바꿀 수 있다.

2. 짝수 번째의 경우 홀수 연결리스트 뒤에 붙일 때 시작 노드를 알고 있어야 하기 때문에 evenStart 변수로 시작 노드를 관리한다.

=> evenStart 보다는 evenHead라는 변수명이 더 직관적이다.

3. 홀수와 짝수에 대한 연결이 끝나면, 짝수 마지막 노드의 next를 null로 바꿔준다. 홀수 노드로 끝날 때, 짝수노드가 마지막 홀수 노드를 가르키고 있기 때문이다.

=> while문에서 홀수와 짝수를 한 번에 처리하면, 짝수로 끝날 때도 마지막 짝수노드의 next가 null이고, 홀수로 끝날 때도 짝수노드의 next가 null이다. 따라서 마지막 처리가 필요없다.

4. head가 null이거나 head.next가 null인 엣지 케이스에 대해서 예외처리를 해준다.

=> while문의 조건을 변경해줬다. even이라는 하나의 변수를 기준으로 간단하게 바꾸어주었고, 이에 따라서 코드 가독성이 높아지고, 예외처리가 줄었다.

5. 두 칸 이후 노드를 `odd = even.next`로 가르킨다.

`odd = odd.next.next`로 변경하면서 두 칸 이후의 노드라는 코드의 의미가 보다 명확해졌다.

# 코드

## 정리 후 코드

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
var oddEvenList = function (head) {
	if (head === null) return null;
	let odd = head;
	let evenHead = head.next;
	let even = head.next;
	while (even !== null && even.next !== null) {
		odd.next = odd.next.next;
		odd = odd.next;
		even.next = even.next.next;
		even = even.next;
	}
	odd.next = evenHead;
	return head;
};
```

## 정리 전 코드

```javascript
var oddEvenList = function (head) {
	if (head === null) return null;
	if (head !== null && head.next === null) return head;
	let i = 0;
	let odd = head;
	let evenStart = head.next;
	let even = head.next;
	while (odd.next !== null && even.next !== null) {
		if (i % 2 === 0) {
			odd.next = even.next;
			odd = odd.next;
		}
		if (i % 2 === 1) {
			even.next = odd.next;
			even = even.next;
		}
		i += 1;
	}
	even.next = null;
	odd.next = evenStart;
	return head;
};
```
