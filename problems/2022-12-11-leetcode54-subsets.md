---
title: LeetCode, Subsets

date: 2022-12-11
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/subsets/)

길이가 N인 각 요소가 unique한 배열이 주어졌을 때, 해당 배열의 요소들로 만들 수 있는 모든 조합을 반환하라.

```javascript
// example
const input = [1, 2, 3];
const output = [[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]];
```

- N의 범위: [1, 10]
- 요소 값의 범위: [-10, 10]

# 문제접근

1. 각 요소에 대해서 "선택한다" or "안 한다" 하는 `분기`에 대해서 `모든 경우의 수`를 구하는 문제이다. 각 분기에 대해서 적어도 한 번 씩은 계산해야하고, 모든 조합의 경우의 수를 구하는 것이기 때문에 O(2^N)이다. (nC0 + nC1 + nC2 + ... nCn)

2. 유향 그래프에서 각 노드가 자신보다 큰 값을 가지는 노드의 방향으로 연결되어 있을 때, 길이와 관계없이 해당 그래프가 가지는 모든 경로를 반환하는 문제로 볼 수 있다.

# 구현 방식

1. `분기`하는 조건과 경우의 수에 대해서 가장 잘 다루는 방식은 재귀함수이다.

2. 하나의 경우를 나타내는 배열(combi)를 만들고, combi에 특정 요소를 넣은 경우, 넣지 않은 경우에 대해서 재귀적으로 함수를 호출한다. 하나의 경우의 수가 만들어진 경우에만 combi를 복사하여 저장하고, 그렇지 않은 경우는 combi 배열을 재활용하여 성능을 높인다.

# 코드

## 경우의 수 개념만으로 푼 코드

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsets = function (nums) {
	const combinations = [];
	const findCombinations = (combi = [], i = nums.length - 1) => {
		if (i < 0) {
			combinations.push([...combi]);
			return;
		}
		findCombinations(combi, i - 1);
		combi.unshift(nums[i]);
		findCombinations(combi, i - 1);
		combi.shift();
	};
	findCombinations();
	return combinations;
};
```

## 그래프 개념으로 푼 코드

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsets = function (nums) {
	const res = [];
	const path = [];
	const dfs = (start) => {
		res.push(path.slice());
		for (let i = start; i < nums.length; i++) {
			path.push(nums[i]);
			dfs(i + 1);
			path.pop();
		}
	};
	dfs(0);
	return res;
};
```
