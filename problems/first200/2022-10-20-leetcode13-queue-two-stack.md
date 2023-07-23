---
title: LeetCode, Implement Queue using Stacks

date: 2022-10-20

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/implement-queue-using-stacks/)

두 개의 stack을 활용하여 queue를 구현하라.

\* stack : data structure that has push, pop, peek, size operation in a LIFO way.

\* queue: data structure that has push, pop, peek, size operation in a FIFO way.

\* queue의 operation의 시간 복잡도가 amortized O(1)이 되도록 하라.

- "amortized(수렴) O(1)": 해당 operation을 여러 번 반복했을 때 총 시간복잡도가 한 operation 당 O(1)이 되어야 한다는 의미이다.

# 문제접근

1. stack의 모든 요소를 pop하여 다른 stack에 push하면 결과적으로 FIFO로 접근하는 것이 된다. push stack을 만들고 pop stack을 만들어서 push stack에 있는 요소를 적절히 pop stack으로 옮기는 방식으로 FIFO로 요소를 접근할 수 있다.

2. pop stack이 비어있을 때, pop이나 peak operation이 일어나면 push stack에 있는 모든 요소를 pop stack으로 옮기기 때문에 항상 O(1)인 것은 아니다. 하지만 결과적으로 한 요소가 push 되어서 pop 될 때까지 딱 한 번만 stack을 옮긴다. 따라서 pop이나 peak의 시간 복잡도가 O(1)으로 수렴한다고 할 수 있다.

# 코드

```javascript
var MyQueue = function () {
	this.pushStack = [];
	this.popStack = [];
};

/**
 * @param {number} x
 * @return {void}
 */
MyQueue.prototype.push = function (x) {
	this.pushStack.push(x);
};

/**
 * @return {number}
 */
MyQueue.prototype.pop = function () {
	this.fillPopStack();
	return this.popStack.pop();
};

/**
 * @return {number}
 */
MyQueue.prototype.peek = function () {
	this.fillPopStack();
	return this.popStack[this.popStack.length - 1];
};

MyQueue.prototype.fillPopStack = function () {
	if (this.popStack.length === 0) {
		while (this.pushStack.length !== 0) {
			this.popStack.push(this.pushStack.pop());
		}
	}
};

/**
 * @return {boolean}
 */
MyQueue.prototype.empty = function () {
	if (this.pushStack.length + this.popStack.length === 0) return true;
	return false;
};
```
