---
title: LeetCode, 215. Kth Largest Element in an Array

date: 2023-1-13

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)

정수가 담긴 배열 nums와 정수 k가 주어진다. 배열에서 k번 째로 큰 수를 반환하라.

- 1 <= k <= nums.length <= 100,000
- -10,000 <= nums[i] <= 10,000

# 문제접근

1. heap이용하여 O(klogN)으로 풀 수 있다. 주어진 배열을 heap을 만족하도록 바꾸는 것은 가장 깊은 레벨의 노드부터 heapDown하는 방식으로 하면 O(N)이다. 그리고 heap에 대해서 k번 pop해야하므로 O(klogN)이다.

2. counting을 활용하여서 O(N)으로 풀 수도 있다. nums 요소의 값의 범위가 그리 넓지 않아서 쓸 수 있다.

# 코드

## heap을 이용한 풀이

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */

class MinHeap {
	initalize(array) {
		this.values = array;
		array.unshift(null);
		for (let i = array.length - 1; i > 0; i--) {
			this.heapDown(i);
		}
	}

	heapDown = (i) => {
		const { values } = this;
		const c1 = i * 2;
		const c2 = i * 2 + 1;
		let largest = i;
		if (c1 < values.length && values[largest] < values[c1]) {
			largest = c1;
		}
		if (c2 < values.length && values[largest] < values[c2]) {
			largest = c2;
		}
		if (largest !== i) {
			[values[largest], values[i]] = [values[i], values[largest]];
			this.heapDown(largest);
		}
	};

	pop() {
		const tmp = this.values[1];
		this.values[1] = this.values[this.values.length - 1];
		this.values.pop();
		this.heapDown(1);
		return tmp;
	}
}

var findKthLargest = function (nums, k) {
	const heap = new MinHeap();
	heap.initalize(nums);
	for (let i = 0; i < k; i++) {
		if (i === k - 1) return heap.pop();
		heap.pop();
	}
};
```

## counting을 이용한 풀이

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */

var findKthLargest = function (nums, k) {
	const min = Math.min(...nums);
	const max = Math.max(...nums);
	const counter = Array(max - min + 1).fill(0);
	for (let i = 0; i < nums.length; i++) {
		counter[nums[i] - min] += 1;
	}
	let p = counter.length - 1;
	while (true) {
		if (k <= counter[p]) return p + min;
		k -= counter[p];
		p--;
	}
};
```
