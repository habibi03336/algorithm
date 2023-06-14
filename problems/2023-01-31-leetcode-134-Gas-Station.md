---
title: LeetCode, 134. Gas Station

date: 2023-1-31

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/gas-station/description/)

n개의 주유소가 원형 서킷에 있다. 각 주유소에서 넣을 수 있는 휘발유의 양이 배열 gas에 담겨 있다. 또한 i번 째 주유소에서 i+1번 째 주유소까지 가는데 필요한 휘발유의 양이 cost에 담겨있다. 처음에 휘발유가 하나도 없는 상태에서 출발해서 원래의 위치로 돌아올 수 있는 주유소를 반환하라. 만약 가능하지 않다면 -1을 반환하라.

- 정답은 1개로 unique하다.

# 문제접근

1. 정답이 하나로 유니크하다는 조건이 문제를 단순화 해준다. 한 주유소에서 휘발유를 넣고 다음 주유소로 갔을 때의 남는, 혹은 부족한 휘발유의 양을 구한다. 첫 번째 반복문에서 시작점의 후보를 찾아준다. 누적이 최대가 되도록 하는 시작점을 찾아줄 수 있다. 그리고 그 누적을 활용하여 한 바퀴를 돌 수 있는지 확인해준다. O(N)이다.

# 코드

```javascript
/**
 * @param {number[]} gas
 * @param {number[]} cost
 * @return {number}
 */
var canCompleteCircuit = function (gas, cost) {
	const lefty = gas.map((g, idx) => g - cost[idx]);
	let cum = 0;
	let start = null;
	for (let i = 0; i < lefty.length; i += 1) {
		cum = lefty[i] + cum;
		if (start === null) start = i;
		if (cum < 0) {
			cum = 0;
			start = null;
		}
	}
	for (let i = 0; i < start; i += 1) {
		cum = lefty[i] + cum;
		if (cum < 0) {
			start = null;
			break;
		}
	}
	return start === null ? -1 : start;
};
```
