---
title: LeetCode, 64. LRU cache

date: 2023-1-5
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/lru-cache/description/)

LRU 캐쉬를 구현하라. LRU(Least Recently Used) 캐쉬는 캐쉬된 데이터를 업데이트할 때 가장 덜 최근에 쓰인 데이터(가장 쓰인지 오래된 데이터)를 내보내는 전략이다.

- LRU 캐쉬 객체를 생성할 때 capacity가 주어진다.
- get과 put은 O(1)으로 동작해야한다.

## 문제접근

1. 얼마나 최근에 쓰였는지에 대한 데이터를 저장해야한다. 배열로 쓰인 순서대로 저장할 수도 있지만, 배열로 저장하게 되면 update할 때 O(N)의 시간이 걸리므로 문제의 조건을 충족하지 못한다.

2. 순서를 관리할 때 Doubly Linked List 자료구조를 활용하면 O(1)으로 순서를 업데이트할 수 있다.

## 구현

1. Doubly Linked List와 Node를 직접구현하여 풀이할 수 있다.

2. javascript Map 객체를 활용할 수도 있다. Map 객채는 get, set, delete와 같은 메소드를 O(1)으로 구현하면서도, set한 순서를 지키는 iterable이기도 한다.

[MDN, Map 객체에 대한 설명](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Keyed_collections)
[크롬 Ordered HashMap의 Map과 Set 구현에 대한 이슈](https://codereview.chromium.org/220293002/)

## 코드

### DLL 자료구조를 활용한 구현

```javascript
function Node(key, value) {
	this.prev = this.next = null;
	this.value = value;
	this.key = key;
}

class DoublyLinkedList {
	constructor() {
		this.head = new Node();
		this.tail = new Node();
		this.head.next = this.tail;
		this.tail.prev = this.head;
	}

	insertHead(node) {
		node.next = this.head.next;
		node.prev = this.head;
		this.head.next.prev = node;
		this.head.next = node;
	}

	removeNode(node) {
		node.next.prev = node.prev;
		node.prev.next = node.next;
	}

	moveToHead(node) {
		this.removeNode(node);
		this.insertHead(node);
	}

	removeLast() {
		const tmp = this.tail.prev;
		this.removeNode(tmp);
		return tmp.key;
	}
}

class LRUCache {
	constructor(capacity) {
		this.dll = new DoublyLinkedList();
		this.cache = {};
		this.capacity = capacity;
		this.curCapacity = 0;
	}

	get(key) {
		if (this.cache[key] === undefined) return -1;
		this.dll.moveToHead(this.cache[key]);
		return this.cache[key].value;
	}

	put(key, value) {
		if (this.cache[key] !== undefined) {
			this.cache[key].value = value;
			this.dll.moveToHead(this.cache[key]);
		} else {
			this.curCapacity++;
			if (this.curCapacity > this.capacity) {
				const removedNodeKey = this.dll.removeLast();
				delete this.cache[removedNodeKey];
				this.curCapacity--;
			}
			const newNode = new Node(key, value);
			this.cache[key] = newNode;
			this.dll.insertHead(newNode);
		}
	}
}
```

### Map 객체를 활용한 구현

```javascript
class LRUCache {
	constructor(capacity) {
		this.map = new Map();
		this.capacity = capacity;
	}

	get(key) {
		const value = this.map.get(key);
		if (value === undefined) return -1;
		this.map.delete(key);
		this.map.set(key, value);
		return value;
	}

	put(key, value) {
		if (this.map.has(key)) {
			this.map.delete(key);
		}
		this.map.set(key, value);
		if (this.map.size > this.capacity) {
			this.map.delete(this.map.keys().next().value);
		}
	}
}
```
