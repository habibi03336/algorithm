---
title: LeetCode, 641. Design Circular Deque

date: 2023-1-19

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/design-circular-deque/description/)

double ended queue (deque)를 구현하라.

# 문제접근

1. cyclic하게 활용한 배열과 포인터를 활용하여 구현할 수 있다.

2. 이중연결 리스트로 구현할 수 있다.

# 코드

## 배열과 포인터를 활용한 구현

118 ms, 50.3 MB

```javascript
/**
 * @param {number} k
 */
var MyCircularDeque = function (k) {
	this.size = k;
	this.array = Array(k).fill(null);
	this.p1 = 0;
	this.p2 = 0;
};

/**
 * @param {number} value
 * @return {boolean}
 */
MyCircularDeque.prototype.insertFront = function (value) {
	if (this.isFull()) return false;
	this.p1 = this.p1 - 1 < 0 ? this.size - 1 : this.p1 - 1;
	this.array[this.p1] = value;
	return true;
};

/**
 * @param {number} value
 * @return {boolean}
 */
MyCircularDeque.prototype.insertLast = function (value) {
	if (this.isFull()) return false;
	this.array[this.p2] = value;
	this.p2 = (this.p2 + 1) % this.size;
	return true;
};

/**
 * @return {boolean}
 */
MyCircularDeque.prototype.deleteFront = function () {
	if (this.isEmpty()) return false;
	this.array[this.p1] = null;
	this.p1 = (this.p1 + 1) % this.size;
	return true;
};

/**
 * @return {boolean}
 */
MyCircularDeque.prototype.deleteLast = function () {
	if (this.isEmpty()) return false;
	this.p2 = this.p2 - 1 < 0 ? this.size - 1 : this.p2 - 1;
	this.array[this.p2] = null;
	return true;
};

/**
 * @return {number}
 */
MyCircularDeque.prototype.getFront = function () {
	if (this.isEmpty()) return -1;
	return this.array[this.p1];
};

/**
 * @return {number}
 */
MyCircularDeque.prototype.getRear = function () {
	if (this.isEmpty()) return -1;
	return this.array[this.p2 - 1 < 0 ? this.size - 1 : this.p2 - 1];
};

/**
 * @return {boolean}
 */
MyCircularDeque.prototype.isEmpty = function () {
	return this.p1 === this.p2 && this.array[this.p2] === null;
};

/**
 * @return {boolean}
 */
MyCircularDeque.prototype.isFull = function () {
	return this.p1 === this.p2 && this.array[this.p2] !== null;
};
```

## 이중연결리스트로 구현

113 ms, 49.6 MB

```javascript
function Node(val, prev, next) {
	this.val = val;
	this.next = next;
	this.prev = prev;
}

/**
 * @param {number} k
 */
var MyCircularDeque = function (k) {
	this.maxSize = k;
	this.size = 0;
	this.head = new Node();
	this.tail = new Node();
	this.head.next = this.tail;
	this.tail.prev = this.head;
};

/**
 * @param {number} value
 * @return {boolean}
 */
MyCircularDeque.prototype.insertFront = function (value) {
	if (this.isFull()) return false;
	this.size += 1;
	const newNode = new Node(value, this.head, this.head.next);
	this.head.next.prev = newNode;
	this.head.next = newNode;
	return true;
};

/**
 * @param {number} value
 * @return {boolean}
 */
MyCircularDeque.prototype.insertLast = function (value) {
	if (this.isFull()) return false;
	this.size += 1;
	const newNode = new Node(value, this.tail.prev, this.tail);
	this.tail.prev.next = newNode;
	this.tail.prev = newNode;
	return true;
};

/**
 * @return {boolean}
 */
MyCircularDeque.prototype.deleteFront = function () {
	if (this.isEmpty()) return false;
	this.size -= 1;
	const tmp = this.head.next;
	this.head.next.next.prev = this.head;
	this.head.next = this.head.next.next;
	return true;
};

/**
 * @return {boolean}
 */
MyCircularDeque.prototype.deleteLast = function () {
	if (this.isEmpty()) return false;
	this.size -= 1;
	const tmp = this.tail.prev;
	this.tail.prev.prev.next = this.tail;
	this.tail.prev = this.tail.prev.prev;
	return true;
};

/**
 * @return {number}
 */
MyCircularDeque.prototype.getFront = function () {
	if (this.isEmpty()) return -1;
	return this.head.next.val;
};
```
