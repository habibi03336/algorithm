---
title: LeetCode, 416. Partition Equal Subset Sum

date: 2023-1-6
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/partition-equal-subset-sum/description/)

정수가 들어있는 길이가 N인 배열 nums가 주어진다. 같은 합을 가지도록 배열의 요소를 둘로 나눌 수 있으면 true 아니면 false를 반환하라.

- nums.length : [1, 200]
- nums\[i\]: [1, 100]

## 문제접근

1. [GeeksForGeeks의 글을 참고 하여서 풀었다.](https://www.geeksforgeeks.org/partition-problem-dp-18/)

2. 모든 경우의 수를 고려하면 O(2^nums.length)이다. 따라서 모든 경우를 고려하는 방식으로 풀기는 어렵다.

3. 메모이제이션을 이용하여, recursion의 중복을 최대한 억제하거나, dp를 활용하여 풀 수 있다. 두 경우 모두 O(Sum(nums) \* nums.length)의 시간복잡도를 가진다.

- 값의 크기가 어느정도 제한되어 있다면 dp풀이를 고려해 볼 수 있다.

- 추가 공부사항: dp의 경우 공간복잡도 최적화를 할 수 있다. 또한 메모이제이션 풀이의 개념으로 set을 활용하여 중복계산을 막는 코드도 찾아 볼 수 있었다. 비트와이스 연산을 활용한 풀이도 있었다.

## 코드

### 메모이제이션을 활용한 풀이

```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canPartition = function (nums) {
	const total = nums.reduce((pre, cur) => pre + cur, 0);
	if (total % 2 !== 0) return false;
	const halfTotal = total / 2;
	const memo = Array(nums.length + 1)
		.fill()
		.map(() => Array(nums.length * 100 + 1).fill(-1));
	const isSubsetSum = (nums, i, sum, memo) => {
		if (sum === 0) return true;
		if (i < 0) return false;
		if (memo[i][sum] !== -1) {
			return memo[i][sum];
		}
		if (nums[i] > sum) {
			return isSubsetSum(nums, i - 1, sum, memo);
		}

		return (memo[i][sum] =
			isSubsetSum(nums, i - 1, sum - nums[i], memo) ||
			isSubsetSum(nums, i - 1, sum, memo));
	};
	return isSubsetSum(nums, nums.length - 1, halfTotal, memo);
};
```

### DP를 활용한 풀이

```javascript
var canPartition = function (nums) {
	const total = nums.reduce((pre, cur) => pre + cur, 0);
	if (total % 2 !== 0) return false;
	const halfTotal = total / 2;
	const maxSum = (nums.length * 100) / 2 + 1;
	const dp = Array(maxSum)
		.fill()
		.map(() => Array(nums.length + 1).fill());
	for (let i = 0; i < nums.length; i++) {
		dp[0][i] = true;
	}
	for (let i = 1; i < maxSum; i++) {
		dp[i][0] = false;
	}
	for (let i = 1; i < maxSum; i++) {
		for (let j = 1; j < nums.length + 1; j++) {
			dp[i][j] = dp[i][j - 1];
			if (i >= nums[j - 1] && dp[i - nums[j - 1]][j - 1] === true) {
				dp[i][j] = true;
			}
		}
	}
	return dp[halfTotal][nums.length];
};
```
