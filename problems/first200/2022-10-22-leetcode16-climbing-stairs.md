---
title: LeetCode, Climbing Stairs

date: 2022-10-22
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/climbing-stairs/submissions/)

올라가야 하는 총 층계의 개수 n이 주어진다. 층계를 하나 혹은 두 개씩 올라 갈 수 있을 때, n까지 올라 가는 경우의 수는 총 몇 개인지 반환하라.

- n은 1 ~ 45

# 문제접근

1. 경우의 수 문제로 접근을 했다.

2step을 한 번도 안 쓰는 경우의 수 + 2step을 한 번만 쓰는 경우의 수 + 2step을 두 번만 쓰는 경우의수 + ... 를 모두 더해서 풀어줄 수 있다.

2. (n번 째까지 올라가는 경우의 수)는 (n-1 번 째까지 올라가서 한 계단 올라가는 경우의 수) + (n-2 번까지 올라가서 두 계단 올라가는 경우의 수)이다. 따라서 피보나치 수열을 구하는 것과 같은 문제가 된다.
	- 관련 문제, 피보나치수: `2023-02-02-leetcode-509-Fibonacci-Number`
# 코드

## 경우의 수 개념으로 풀기

```javascript
/**
 * @param {number} n
 * @return {number}
 */
const combination = (n, k) => {
	let no = 1;
	for (let i = n; i > n - k; i -= 1) {
		no *= i;
	}
	let de = 1;
	for (let i = 1; i <= k; i += 1) {
		de *= i;
	}
	return no / de;
};

var climbStairs = function (n) {
	let cnt = 0;
	let nn = n;
	let n2 = 0;
	while (nn >= n2) {
		cnt += combination(nn, n2);
		nn -= 1;
		n2 += 1;
	}
	return cnt;
};
```

## 피보나치 문제로 풀기

```javascript
const dp = Array(46).fill(null);
var climbStairs = function (n) {
	if (n < 3) return n;
	if (dp[n] !== null) return dp[n];
	dp[n] = climbStairs(n - 2) + climbStairs(n - 1);
	return dp[n];
};
```
