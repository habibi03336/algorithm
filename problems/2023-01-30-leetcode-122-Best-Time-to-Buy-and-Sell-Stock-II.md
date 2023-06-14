---
title: LeetCode, 122. Best Time to Buy and Sell Stock II

date: 2023-1-30

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)

주식의 가격이 시간 순으로 담긴 배열이 주어진다. 하나의 주식을 여러번 사고 팔 때, 극대화한 수익은 얼마인가?

# 문제접근

1. 그리디한 방식으로 풀 수 있다. 하루 단위로 이익이 있을 때마다 사고 팔았다고 하면 이익이 극대화 된다.

# 코드

```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function (prices) {
	let profit = 0;
	for (let i = 1; i < prices.length; i += 1) {
		if (prices[i] > prices[i - 1]) {
			profit += prices[i] - prices[i - 1];
		}
	}
	return profit;
};
```
