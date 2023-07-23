---
title: LeetCode, Contains Duplicate

date: 2022-10-24
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/contains-duplicate/)

길이가 n인 정수 array가 주어졌을 때, 만약 한 번이라도 중복되는 요소가 있는 경우 true, 아니면 false를 반환하라.

- n은 1 ~ 10,000
- -10^9 < array의 요소 < 10^9 (이렇게 범위가 넓으면 array와 index를 활용한 요소 체크가 어렵다.)

# 문제접근

1. Set 객체(집합 개념)를 활용하여, 요소의 중복을 확인해줄 수 있다.

# 코드

```javascript
var containsDuplicate = function (nums) {
	const bag = new Set();
	for (let i = 0; i < nums.length; i++) {
		if (bag.has(nums[i])) return true;
		bag.add(nums[i]);
	}

	return false;
};
```
