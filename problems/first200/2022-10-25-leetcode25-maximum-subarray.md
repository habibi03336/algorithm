---
title: LeetCode, Maximum Subarray

date: 2022-10-25
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/maximum-subarray/)

정수로 이루어진 길이가 n인 array가 주어졌을 때, 연속적인 subarray 요소의 총합 중 가장 큰 값을 반환하라.

- subarray는 적어도 하나의 요소를 포함한다.
- n은 1 ~ 10,000

# 문제접근

1. 각 경우에 대해서 최적값을 저장하는 카데인 알고리즘으로 풀어줄 수 있다. i번 째를 마지막으로 포함하는 subarray를 만든다고 할 때, 앞 선 요소들의 총합이 양수이면 포함하고, 음수이면 제외하는 것이 총합을 최대로 만든다. i의 0번째 부터 n-1번 째까지 모든 경우의 수에 대해서 최적값을 반환하면 된다.

2. 다이나믹 프로그래밍의 개념으로 접근할 수 있다. (0부터 i까지의 최대 subarray 합)은 (0부터 i-1까지 최대 subarray 합 + i번 째 값) or (i번 째 값)이다. 따라서 최적 부분 구조라고 할 수 있다.

# 코드

```javascript
var maxSubArray = function (nums) {
	let localSum = 0;
	let max = nums[0];
	for (let i = 0; i < nums.length; i++) {
		localSum += nums[i];
		if (localSum > max) max = localSum;
		if (localSum < 0) localSum = 0;
	}

	return max;
};
```

```javascript
var maxSubArray = function (nums) {
	const maxs = Array(nums.length).fill(null);
	maxs[0] = nums[0];
	for (let i = 1; i < nums.length; i += 1) {
		maxs[i] = Math.max(nums[i], maxs[i - 1] + nums[i]);
	}
	return Math.max(...maxs);
};
```
