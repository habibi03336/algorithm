---
title: LeetCode, Insert Interval

date: 2022-10-26
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/insert-interval/)

서로 overlapping 되지 않는 interval의 start, end를 담은 array의 array가 주어진다. interval은 start를 기준으로 오름차순으로 정렬되어있다.(앞 선 interval의 end는 다음 interval의 start 보다 항상 작다.)

또 하나의 interval이 따로 주어진다.

interval의 array에 따로 주어진 interval을 기존의 조건(overlap이 없고, 오름차순)에 만족하도록 합쳐라. 만약 기존의 interval과 ovelap되는 부분이 있으면 interval을 합쳐라.

- interval의 start는 end와 크거나 같다.
- interval array의 length는 0 ~ 10,000이다.

# 문제접근

1. linear하게 intervals를 조회하면서 insert할 곳을 찾아주고, 만약 overlap되는 부분이 있다면 잘 합쳐서 새로운 interval을 만들어 넣어주면 된다.

- 생각만큼 간단하게 풀리지는 않았다. 경우의 수가 생각보다 다양했다. overlap이 되는 경우(4가지 경우) + overlap 되지 않고 들어가는 경우
<div style="text-align: center;">
    <img src="/assets/img/leet-overlap.jpeg" alt="overlap cases" width="400"/>
</div>

# 코드

```javascript
/**
 * @param {number[][]} intervals
 * @param {number[]} newInterval
 * @return {number[][]}
 */
var insert = function (intervals, newInterval) {
	const result = [];
	let isInserted = false;
	let i = 0;
	while (i < intervals.length) {
		if (intervals[i][1] < newInterval[0]) {
			result.push(intervals[i]);
			i++;
		} else {
			isInserted = true;
			const mergedInterval = [];
			if (newInterval[0] < intervals[i][0]) {
				mergedInterval.push(newInterval[0]);
			} else {
				mergedInterval.push(intervals[i][0]);
			}
			while (i < intervals.length && intervals[i][0] <= newInterval[1]) {
				i++;
			}
			if (i - 1 < 0 || newInterval[1] > intervals[i - 1][1]) {
				mergedInterval.push(newInterval[1]);
			} else {
				mergedInterval.push(intervals[i - 1][1]);
			}
			result.push(mergedInterval);
			break;
		}
	}

	while (i < intervals.length) {
		result.push(intervals[i]);
		i++;
	}

	if (!isInserted) {
		result.push(newInterval);
	}

	return result;
};
```
