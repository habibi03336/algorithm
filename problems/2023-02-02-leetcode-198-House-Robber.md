---
title: LeetCode, 198. House Robber

date: 2023-2-2

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/house-robber/description/)

도둑질을 하려고 한다. 집에 있는 훔질 물건의 양이 담긴 배열 nums가 주어진다. 다만, 두 집을 연속하여 도둑질 할 수 없다. 도둑질 하여 얻을 수 있는 최대 물건의 양을 반환하라.

- 1 <= nums.length <= 100

# 문제접근

1. (0번 부터 k번 집까지 최대로 훔치는 양)은 (0번 부터 k-1번 집까지 최대로 훔치는 양)과 (0번 부터 k-2번 집까지 최대로 훔치는 양 + k번 째 집 훔치는 양)이다. 따라서 더 작은 문제의 최적을 구해가는 문제인 최적 부분 문제로 볼 수 있다. 이를 활용하여 다이나믹 프로그래밍으로 풀어줄 수 있다.

- (k-1번까지 털고 k를 털지 않는 경우), (k-2번까지 털고 k를 터는 경우) 둘 중에 큰 것이 (k까지 터는 경우)의 최대값이다.

# 코드

## 다이나믹 프로그래밍

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function (nums) {
	const dp = Array(nums.length).fill(null);
	dp[0] = nums[0];
	dp[1] = Math.max(nums[0], nums[1]);
	for (let i = 2; i < nums.length; i += 1) {
		dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
	}
	return dp[nums.length - 1];
};
```

## 공간최적화

```javascript
var rob = function (nums) {
	if (nums.length === 1) return nums[0];
	let a = nums[0];
	let b = Math.max(nums[0], nums[1]);
	for (let i = 2; i < nums.length; i += 1) {
		const tmp = b;
		b = Math.max(a + nums[i], b);
		a = tmp;
	}
	return b;
};
```
