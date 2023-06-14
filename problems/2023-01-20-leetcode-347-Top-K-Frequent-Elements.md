---
title: LeetCode, 347. Top K Frequent Elements

date: 2023-1-20

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/top-k-frequent-elements/description/)

정수를 담은 길이가 n인 배열이 주어진다. 또한 자연수 k가 주어진다. 배열에서 k 번째까지 많은 빈도수의 요소를 반환하라.

- 답은 unique 하다.(같은 빈도수 때문에 답이 갈리는 경우는 없다.)
- k의 범위: [1, unique한 배열 요소의 종류]

# 문제접근

1. 배열의 요소를 카운팅한 후, 카운팅 값을 기준으로 정렬하여 k번째 요소까지 반환하면 된다.

# 코드

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var topKFrequent = function (nums, k) {
	const count = {};
	for (let num of nums) {
		if (count[num] === undefined) count[num] = 1;
		else count[num] += 1;
	}
	const arr = Object.entries(count);
	arr.sort((a, b) => b[1] - a[1]);
	const res = [];
	for (let i = 0; i < k; i++) {
		res.push(arr[i][0]);
	}
	return res;
};
```
