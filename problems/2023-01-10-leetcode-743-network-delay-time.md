---
title: LeetCode, 743. Network Delay Time

date: 2023-1-10

categories:
  - 문제풀이

tags:
  - Exercise
  - 🧑🏻‍💻
---

# [문제요약](https://leetcode.com/problems/network-delay-time/description/)

(시작노드, 끝노드, 가중치)를 요소로하는 배열 times가 주어진다. 또한 노드의 개수를 나타내는 n이 주어진다. 1부터 n까지의 노드가 있음을 의미한다. 신호가 시작되는 노드 k가 주어질 때, 모든 노드에 신호가 전달되는 가장 빠른 시점을 반환하라. 만약 모든 노드에 전달 될 수 없으면 -1을 반환하라.

# 문제접근

1. 다익스트라 알고리즘을 활용하여 풀 수 있다. 각 노드에 도달하는 가장 작은 비용을 구하고, 그 비용 중 가장 큰 것을 반환하면 된다.

2. 다익스트라 알고리즘을 효율적으로 구현하기 위해서는 heap 자료구조를 활용하는 것이 좋다. 다익스트라 알고리즘을 활용하기 위해서는 가장 작은 비용으로 갈 수 있는 노드를 지속적으로 탐색해야한다. 선형 자료구조를 사용하면 insert+탐색이 O(N)이지만 heap을 사용하면 O(logN)이다.

# 구현

1. 자바스크립트는 보통 heap 자료구조를 내장하고있지 않다. 따라서 직접 heap을 구현해주어야 한다.

2. 가장 작은 비용으로 갈 수 있는 노드를 뽑아서, 그 노드의 인접노드를 방문하면서 heap에 insert해준다. 항상 가장 작은 비용으로 갈 수 있는 노드만을 뽑는다. 따라서 (비용이 음수가 아닌이상) 이후에 더 작은 비용이 나올 수는 없다. 그렇기 때문에 노드와 비용을 바로 기록하고, 이후에 같은 노드가 뽑히는 경우 무시하면 된다.

# 코드

```javascript
/**
 * @param {number[][]} times
 * @param {number} n
 * @param {number} k
 * @return {number}
 */

class MinHeap {
	constructor() {
		this.values = [];
	}

	getSize() {
		return this.values.length;
	}

	swap(a, b) {
		const tmp = this.values[a];
		this.values[a] = this.values[b];
		this.values[b] = tmp;
	}

	bubbleUp() {
		const { values } = this;
		let idx = this.values.length - 1;
		let parent = ((idx - 1) / 2) >> 0;
		while (idx !== 0 && values[idx][0] < values[parent][0]) {
			this.swap(idx, parent);
			idx = parent;
			parent = ((idx - 1) / 2) >> 0;
		}
	}

	bubbleDown() {
		const { values } = this;
		let idx = 0;
		let [left, right] = [idx * 2 + 1, idx * 2 + 2];
		while (left < values.length) {
			let smallestChild = left;
			if (right < values.length && values[right][0] < values[left][0]) {
				smallestChild = right;
			}
			if (values[idx][0] > values[smallestChild][0]) {
				this.swap(idx, smallestChild);
				idx = smallestChild;
				left = idx * 2 + 1;
				right = idx * 2 + 2;
				continue;
			}
			break;
		}
	}

	insertion(value) {
		this.values.push(value);
		this.bubbleUp();
	}

	deletion() {
		const tmp = this.values[0];
		this.values[0] = this.values[this.values.length - 1];
		this.values.pop();
		this.bubbleDown();
		return tmp;
	}
}

var networkDelayTime = function (times, n, k) {
	const adjMap = {};
	for (let i = 0; i < times.length; i++) {
		const time = times[i];
		if (adjMap[time[0]] === undefined) {
			adjMap[time[0]] = [[time[2], time[1]]];
		} else {
			adjMap[time[0]].push([time[2], time[1]]);
		}
	}

	const heap = new MinHeap();
	const dist = {};
	heap.insertion([0, k]);
	while (heap.getSize() !== 0) {
		const [time, node] = heap.deletion();
		if (!(node in dist)) {
			dist[node] = time;
			if (adjMap[node] === undefined) continue;
			for (let [w, v] of adjMap[node]) {
				const tw = w + time;
				heap.insertion([tw, v]);
			}
		}
	}
	const distVals = Object.values(dist);
	if (distVals.length !== n) return -1;
	return Math.max(...distVals);
};
```
