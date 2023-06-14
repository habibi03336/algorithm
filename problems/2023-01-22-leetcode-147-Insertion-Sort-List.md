---
title: LeetCode, 147. Insertion Sort List

date: 2023-1-22

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/insertion-sort-list/description/)

연결 리스트가 주어진다. 연결 리스트를 삽입 정렬 알고리즘으로 정렬하라.

# 문제접근

1. 삽입 정렬은 순차적으로 요소를 조회하고, 앞서 정렬된 요소들과 비교하여 정렬하는 방식이다. 완전히 역순으로 되어있는 경우가 최악의 경우고 시간복잡도는 O(N^2)이다. 완전히 정렬되어 있는 경우가 최선이고 O(N)이다.

# 구현

1. 비교하는 대상(cur)을 항상 처음으로 초기화하는 것이 아니라, 새로운 정렬 대상(target)의 값보다 비교 대상이 큰 경우만 초기화 해주는 방식으로 최적화 할 수 있다.

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
var insertionSortList = function (head) {
	let cur = new ListNode(0);
	const parent = cur;
	let target = head;
	while (target !== null) {
		while (cur.next && target.val > cur.next.val) {
			cur = cur.next;
		}
		[cur.next, target.next, target] = [target, cur.next, target.next];
		if (target && target.val < cur.val) cur = parent;
	}
	return parent.next;
};
```
