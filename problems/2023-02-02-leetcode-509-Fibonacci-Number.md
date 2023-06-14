---
title: LeetCode, 509. Fibonacci Number

date: 2023-2-2

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/fibonacci-number/description/)

n 번 째 피보나치 수를 반환하여라.

# 문제접근

1. 다이나믹 프로그래밍 입문 문제로 브루트포스, 메모이제이션, 타뷸레이션 각각의 방법으로 풀어보았다.

# 코드

## 재귀 브루트포스

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var fib = function (n) {
	if (n < 2) return n;

	return fib(n - 1) + fib(n - 2);
};
```

## 메모이제이션

```javascript
/**
 * @param {number} n
 * @return {number}
 */
const dp = Array(31).fill(null);
var fib = function (n) {
	if (n < 2) return n;

	if (dp[n] !== null) return dp[n];

	dp[n] = fib(n - 1) + fib(n - 2);
	return dp[n];
};
```

## 타뷸레이션

```javascript
var fib = function (n) {
	if (n < 2) return n;
	const dp = Array(31).fill(null);
	dp[0] = 0;
	dp[1] = 1;
	for (let i = 2; i <= n; i += 1) {
		dp[i] = dp[i - 1] + dp[i - 2];
	}
	return dp[n];
};
```
