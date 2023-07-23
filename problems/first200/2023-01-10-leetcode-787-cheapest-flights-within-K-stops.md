---
title: LeetCode, 787. Cheapest Flights Within K Stops

date: 2023-1-10

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/cheapest-flights-within-k-stops/description/)

노드의 개수를 나타내는 n과, (시작노드, 끝노드, 가중치)를 요소로하는 배열 flights 주어진다. 시작점 src에서 도착점 dst로 가는데 k개 이하의 노드만 경유에서 가는 길 중에 가장 작은 비용을 반환하라. 불가능하면 -1을 반환하라.

# 문제접근

1. 다익스트라 알고리즘을 응용하여 풀 수 있다. 다익스트라 알고리즘의 경우 원래 가장 짧은 경로에 대해서만 그 이후 경로를 heap에 넣어준다. 하지만 이 문제에서는 경유조건 때문에 비교적 긴 경로가 선택되는 경우가 있다. 따라서 가장 짧은 경로가 아니더라도 경유 조건을 만족하는 경우에는 heap에 넣어준다. 이렇게하면 여전히 가장 가까운 경로를 우선적으로 탐색하지만, 가장 짧은 경로가 아니더라도 heap에 보관되어 차례를 기다리게 된다.

# 구현

1. 메모리 부족 문제가 발생했다. 경로 조건이 느슨한 경우에 너무 heap에 너무 많은 경로가 저장된다. 경로 조건이 느슨하기 때문에 거의 brute force 탐색하듯이 경우의 수가 만들어진다. 공간 복잡도가 O(n^k)이다. 따라서 이를 제어해주는 것이 필요했다. 적어도 시간 혹은 경유한 수 둘 중에 하나에서라도 이전보다 우위가 있는 경우에만 다음 경로를 생성하도록 했다.

# 코드

```javascript
/**
 * @param {number} n
 * @param {number[][]} flights
 * @param {number} src
 * @param {number} dst
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

var findCheapestPrice = function (n, flights, src, dst, k) {
	const adjMap = {};
	for (let i = 0; i < flights.length; i++) {
		const f = flights[i];
		if (adjMap[f[0]] === undefined) {
			adjMap[f[0]] = [[f[2], f[1]]];
		} else {
			adjMap[f[0]].push([f[2], f[1]]);
		}
	}

	const dist = {};
	const heap = new MinHeap();
	heap.insertion([0, src, -1]);
	while (heap.getSize() !== 0) {
		const [time, node, via] = heap.deletion();
		if (node === dst) return time;
		if (via < k) {
			if (
				dist[node] !== undefined &&
				dist[node][0] <= time &&
				dist[node][1] <= via
			) {
				continue;
			}
			dist[node] = [
				Math.max(dist[node] ? dist[node][0] : time, time),
				Math.max(dist[node] ? dist[node][1] : via, via),
			];
			if (adjMap[node] === undefined) continue;
			for (let [w, v] of adjMap[node]) {
				const tw = w + time;
				heap.insertion([tw, v, via + 1]);
			}
		}
	}
	return -1;
};
```
