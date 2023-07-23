---
title: LeetCode, 771. Jewels and Stones

date: 2023-1-20

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/jewels-and-stones/description/)

보석 종류를 나타내는 문자열 jewels와 돌을 나타내는 문자열 stones가 주어진다. 돌 중에서 보석인 것의 개수를 반환하라.

- jewels의 문자는 unique하다.

# 문제접근

1. 각각의 돌이 jewel에 해당하는지 확인해주면서 카운팅 해줄 수 있다.

2. 돌을 종류별로 미리 카운팅 해놓고, 보석인 경우 개수를 더해줄 수 있다.

# 코드

## 보석인 경우 카운팅 하는 방식

```javascript
/**
 * @param {string} jewels
 * @param {string} stones
 * @return {number}
 */
var numJewelsInStones = function (jewels, stones) {
	const jewelSet = new Set(jewels);
	let cnt = 0;
	for (let stone of stones) {
		if (jewelSet.has(stone)) cnt += 1;
	}
	return cnt;
};
```

## 미리 카운팅 해놓고 보석인 경우 더해주는 방식

```javascript
var numJewelsInStones = function (jewels, stones) {
	const counts = {};
	for (let stone of stones) {
		if (counts[stone] === undefined) {
			counts[stone] = 1;
		} else {
			counts[stone] += 1;
		}
	}
	let cnt = 0;
	for (let jewel of jewels) {
		if (counts[jewel] === undefined) continue;
		cnt += counts[jewel];
	}
	return cnt;
};
```
