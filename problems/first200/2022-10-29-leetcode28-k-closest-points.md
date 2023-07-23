---
title: LeetCode, K Closest Points to Origin

date: 2022-10-29
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/k-closest-points-to-origin/)

2차원 평면의 포인트 (x,y)가 array로 주어진다. 또 자연수 k가 주어진다. 원점에서 포인트까지 거리가 가장 짧은 k개의 포인트를 array로 반환하라. 반한하는 array의 순서는 상관 없다.

- 답은 unique하다.(같은 거리 때문에 여러 개의 답이 나오는 경우는 없다.)
- 포인트의 개수는 1 ~ 10,000

# 문제접근

1. 거리를 기준으로 sorting하여 가장 작은 k개를 반환하면 된다.

- O(NlogN)

2. Heap을 활용하여 가장 작은 k개를 선택할 수 있다.

- O(Nlogk) (힙에는 k개의 노드만 있다.)

# 코드

## sorting을 이용한 풀이

```javascript
var kClosest = function (points, k) {
	points.sort((a, b) => a[0] ** 2 + a[1] ** 2 - (b[0] ** 2 + b[1] ** 2));
	return points.slice(0, k);
};
```

## Heap을 이용한 풀이

```javascript
class Heap {
	constructor(comparisonLogic, size) {
		this.heap = [];
		this.comparisonLogic = comparisonLogic;
		this.heapSize = size || Infinity;
	}

	getParentIndex(index) {
		return Math.floor((index - 1) / 2);
	}

	getLeftChildIndex(index) {
		return index * 2 + 1;
	}

	getRightChildIndex(index) {
		return index * 2 + 2;
	}

	hasLeftChild(index) {
		return this.heap[this.getLeftChildIndex(index)] !== undefined
			? true
			: false;
	}

	hasRightChild(index) {
		return this.heap[this.getRightChildIndex(index)] !== undefined
			? true
			: false;
	}

	hasParent(index) {
		return this.heap[this.getParentIndex(index)] !== undefined ? true : false;
	}

	getHeapValue() {
		return this.heap.length === 0 ? null : this.heap[0];
	}

	getCurrentHeapSize() {
		return this.heap.length;
	}

	insertValue(value) {
		if (this.getCurrentHeapSize() >= this.heapSize) {
			this.removeValue();
		}
		this.heap.push(value);
		this.heapifyUp();
	}

	removeValue() {
		if (this.heap.length === 0) {
			return null;
		}
		if (this.heap.length === 1) {
			return this.heap.pop();
		}

		this.heap[0] = this.heap.pop();
		this.heapifyDown();
	}

	swapValue(index1, index2) {
		const value = this.heap[index1];
		this.heap[index1] = this.heap[index2];
		this.heap[index2] = value;
	}

	heapifyUp() {
		let index = this.heap.length - 1;

		while (
			this.hasParent(index) &&
			this.comparisonLogic(
				this.heap[this.getParentIndex(index)],
				this.heap[index]
			)
		) {
			const parentIndex = this.getParentIndex(index);
			this.swapValue(index, parentIndex);
			index = parentIndex;
		}
	}

	heapifyDown() {
		let index = 0;

		while (this.hasLeftChild(index)) {
			let currentIndex = index;
			if (
				this.comparisonLogic(
					this.heap[currentIndex],
					this.heap[this.getLeftChildIndex(index)]
				)
			) {
				currentIndex = this.getLeftChildIndex(index);
			}

			if (
				this.hasRightChild(index) &&
				this.comparisonLogic(
					this.heap[currentIndex],
					this.heap[this.getRightChildIndex(index)]
				)
			) {
				currentIndex = this.getRightChildIndex(index);
			}
			this.swapValue(index, currentIndex);
			if (index === currentIndex) {
				break;
			}
			index = currentIndex;
		}
	}
}

/**
 * @param {number[][]} points
 * @param {number} k
 * @return {number[][]}
 */
var kClosest = function (points, k) {
	const myHeap = new Heap((pointA, pointB) => {
		return pointA.distance < pointB.distance;
	}, k);

	for (let point of points) {
		const distance = Math.sqrt(
			Math.pow(point[0] - 0, 2) + Math.pow(point[1] - 0, 2)
		);
		if (
			myHeap.getCurrentHeapSize() >= k &&
			myHeap.heap[0].distance < distance
		) {
			continue;
		}
		myHeap.insertValue({
			distance,
			point,
		});
	}

	return myHeap.heap.map(({ point }) => point);
};
```
