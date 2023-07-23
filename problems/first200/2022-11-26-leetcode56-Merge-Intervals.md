---
title: LeetCode, 56. Merge Intervals

date: 2022-11-26

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/merge-intervals/)

1차원 좌표의 구간의 시작과 끝이 주어진다.

ex) \[ \[1, 3\], \[4, 6\], \[5, 10\] \]

구간이 겹치는 구간이 있으면 합쳐서 겹치는 구간이 없도록 하여 반환하라.

ex) \[ \[1, 3\], \[4, 10\] \]

- 구간의 시작점 <= 구간의 끝점
- 구간 개수의 범위: \[1, 10,000\]


# 문제접근

1. 구간의 시작점을 기준으로 먼저 정렬(O(NlogN))을 해주면 복잡도가 낮아진다. 먼저 조회된 구간의 시작점이 항상 더 작음을 알 수 있다. 또한 구간의 끝점이 다음 구간 시작점보다 작으면 더 이상 겹치는 부분이 없음을 확신할 수 있다.

2. 구간의 끝점이 다음 구간 시작점보다 크면 겹치는 것이다. 이때 두 구간을 합쳐 새로운 하나의 구간을 만들어야 한다. 합쳐진 구간의 시작점은 항상 먼저 조회된 구간이 시작점이다. 항상 먼저 조회된 구간의 시작점이 더 작기 때문이다. 하지만 끝점의 경우 두 구간 중 어느 구간의 것이 더 큰지 확신 할 수 없다. 따라서 둘 중 더 큰 쪽으로 업데이트 해주어야 한다.

3. 관련문제: `2022-10-26-leetcode26-insert-interval`

# 코드

## 단순화된 코드

```javascript
/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */
var merge = function (intervals) {
	intervals.sort((a, b) => a[0] - b[0]);
	const res = [];
	for (let elem of intervals) {
		if (res.length !== 0 && res[res.length - 1][1] >= elem[0]) {
			res[res.length - 1][1] = Math.max(res[res.length - 1][1], elem[1]);
		} else {
			res.push(elem);
		}
	}
	return res;
};
```

## 처음 코드

```javascript
/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */
var merge = function (intervals) {
	intervals.sort((a, b) => a[0] - b[0]);
	const results = [];
	let [start, end] = intervals[0];
	for (let i = 1; i < intervals.length; i++) {
		const [nextStart, nextEnd] = intervals[i];
		if (end < nextStart) {
			results.push([start, end]);
			start = nextStart;
			end = nextEnd;
			continue;
		}
		if (end >= nextStart && nextEnd > end) end = nextEnd;
	}
	results.push([start, end]);
	return results;
};
```
