---
title: LeetCode, 136. Single Number

date: 2023-1-24

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/single-number/description/)

정수가 담긴 배열 nums가 주어진다. nums는 한 번만 등장하는 하나의 요소와 두 번씩 등장하는 요소 쌍으로 이루어져있다. 한 번만 등장하는 요소의 값을 반환하라.

- nums\[i\]의 범위: [-30,000, 30,000]
- O(N)의 시간복잡도와 O(1)의 공간복잡도로 풀이해야한다.

# 문제접근

1. 비트연산을 활용하여 풀 수 있다. 32bit int형에서 각 비트 단위로 생각을 해볼 수 있다. 0에서 시작해서 XOR 연산을 해주면 1이 홀수번 등장하는 비트에만 1이 표시된다.

# 코드

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function (nums) {
	let record = 0;
	for (let i = 0; i < nums.length; i += 1) {
		record = record ^ nums[i];
	}
	return record;
};
```
