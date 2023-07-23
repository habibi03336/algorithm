---
title: LeetCode, Linked List Cycle

date: 2022-10-18

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/linked-list-cycle/)

singly linked list가 있다. 만약 한 노드가 뒤에 이어진 노드의 next 포인터로 다시 접근 될 수 있으면 이를 "Linked List Cycle"이라고 한다. singly linked list의 head가 주어질 때 이 리스트가 cycle인지 아닌지 반환하라.

- 노드의 개수: [0, 10^4]

# 문제접근

1. 노드 개수 제한을 활용해서 풀 수 있다. 문제에서 노드의 최대 개수는 10,000개이다. cycle이 아닌 경우 최대 10,000번 째 노드까지 있을 수 있고, 10,001번 째 노드는 반드시 null이다. 반면에
cycle인 경우 무한루프이기 때문에 10,001번째 노드는 반드시 null이 아니다. 이를 활용하여 메모리 O(1), 시간복잡도 O(Max(N))로 풀 수 있다.

2. 두 번째 접근은 node에 visited 프로퍼티를 만드는 것이다. visited 된 노드를 다시 한 번 방문한다면 cycle이라고 판단 할 수 있다. 메모리, 시간 복잡도 모두 O(N)이다.

3. 다른 사람의 풀이를 보니 walker와 runner라는 개념을 통해 푼 것을 발견했다. 노드를 한 개씩 넘어가는 walker와 두 개씩 넘어가는 runner를 설정하고, runner가 결국 null이 되면 non-cycle이고, runner가 walker를 따라잡으면 cycle이라고 판단하는 방식이다. (시간복잡도: O(N), 공간복잡도: O(1))

- 이 풀이에서 조금 궁금했던 것이 cycle일 때 runner와 walker가 결국 한 노드에서 만난다는 것을 보장할 수 있는가? 였다. 연역적으로 이를 확인할 수 있다.
  1. runner는 walker를 추월할 수 밖에 없다.
  2. runner가 walker를 추월하는 단계에서 runner는 walker보다 딱 한 노드 앞에 있게 된다.
  3. runner가 walker 딱 한 노드 앞에 있다는 것은 그 전 단계에서 같은 노드에 있었다는 것을 의미한다.

# 코드

## 첫 번째 접근

117 ms, 44.8 MB

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function (head) {
	let node = head;
	for (let i = 0; i < 10 ** 4 + 1; i++) {
		if (node === null) return false;
		node = node.next;
	}

	return true;
};
```

## 두 번째 접근

127 ms, 45.3 MB

```javascript
var hasCycle = function (head) {
	let node = head;
	while (node) {
		if (node.visited) return true;
		node.visited = true;
		node = node.next;
	}

	return false;
};
```

## 세 번째 접근

107 ms, 45.3 MB

```javascript
var hasCycle = function (head) {
	let walker = head;
	let runner = head?.next;
	while (runner) {
		if (runner === walker) return true;
		walker = walker.next;
		runner = runner.next?.next;
	}

	return false;
};
```
