---
title: LeetCode, Min Stack

date: 2022-11-17
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/min-stack/)

Min Stack 자료구조를 구현하라. Min Stack에는 stack의 push, pop, top operation과 getMin operation이 있다. 각 operation은 O(1)의 시간복잡도를 가져야한다.

- 주어지는 value의 범위: [-2\*\*31, 2\*\*31 - 1]
- pop, top, getMin operation은 stack이 비어있을 때 호출되지 않음.

# 문제접근

1. push, pop, top, getMin operation이 모두 O(1)의 시간복잡도를 가져야한다는 것이 어려운 지점이었다. [Min Stack은 보편적으로 알려진 자료구조 중 하나이다.](https://www.geeksforgeeks.org/design-a-stack-that-supports-getmin-in-o1-time-and-o1-extra-space/)

2. 여태까지의 가장 작은 값을 저장해 놓는다. 이 저장 값을 통해 O(1)으로 getMin을 구현할 수 있다. 문제는 stack에 있는 값들이 push와 pop 메소드를 통해 동적으로 바뀌는 상황에서 어떻게 저장된 가장 작은 값을 동기화 시켜주냐이다. 만약 기존 가장 작은 값(A)보다 더 작은 값(B)이 push되면 stack에는 (2\*B - A) 을 넣는다. 그리고 mininum 저장 값을 B로 갱신한다. (2\*B - A)값은 필연적으로 갱신된 minimum 값(B)보다 작다. 왜냐하면 B < A 는 2\*B - A < B 이기 때문이다. 그렇다면 만약 pop operation이 일어날 때 stack에서 나온 값이 저장된 min값(B)보다 작으면 해당 value가 가장 작은 것임을 알 수 있다. 따라서 그런 경우 pop operation이 일어나면 저장된 min 값(B)을 갱신해야한다. 이때 pop된 값(2\*B - A), 저장된 min 값을 활용하여 이전에 저장되 있던 min 값을 다시 만들 수 있다. 이전에 저장되어 있던 min 값(A)은 현재 저장되어있는 min 값(B) \* 2 - pop된 값(2\*B - A)이다.

> (ex) 예를 들어 8, 3, 1 순서로 push되었다고 해보자.  
> 8 push => min: 8, stack: [(2\*8 - defaultMinValue)],  
> 3 push => min: 3, stack: [(2\*8 - defaultMinValue), (2*3 - 8)]  
> 2 push => min: 2, stack: [(2\*8 - defaultMinValue), (2\*3 - 8), (2\*2 -3)]

3. stack 자료구조이기 때문에(LIFO)이기 때문에 위와 같이 min 값을 관리할 수 있다. LIFO이기 때문에 값이 pop될 때 순차적으로 min value를 갱신해줄 수 있다.

# 코드

```javascript
var MinStack = function () {
	this.MAX_NUMBER = 2 ** 31 + 1;
	this.min = this.MAX_NUMBER;
	this.stack = [];
};

/**
 * @param {number} val
 * @return {void}
 */
MinStack.prototype.push = function (val) {
	let insertVal = val;
	if (val < this.min) {
		insertVal = 2 * val - this.min; // 2B - A
		this.min = val;
	}
	this.stack.push(insertVal);
	return null;
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function () {
	const popped = this.stack.pop();

	// 2B - A < B
	if (popped < this.min) {
		this.min = 2 * this.min - popped; // 2B - 2B + A
	}
	return null;
};

/**
 * @return {number}
 */
MinStack.prototype.top = function () {
	return this.stack[this.stack.length - 1] < this.min
		? this.min
		: this.stack[this.stack.length - 1];
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function () {
	return this.min;
};

/**
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(val)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```
