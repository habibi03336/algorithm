---
title: LeetCode, 121. Best Time to Buy and Sell Stock

date: 2022-10-11

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

길이가 N인 array가 주어진다. array의 요소는 시간에 따라 변한 주식가격을 의미한다. 주식을 하나 구매했다가 팔았을 때 낼 수 있던 가장 큰 이익은 얼마인지 구하려라. (이익을 낼 수 없는 경우 0을 반환하라.)

- N은 100,000

# 문제접근

1. 정해진 시점까지 가장 저렴 할 때를 기준으로, 해당 시점까지 낼 수 있는 최대의 이익이 얼마인지 기록해주는 방식으로 풀 수 있다.(O(N))

- 다른 말로 로컬 최적을 구하고, 로컬 최적 중에서 가장 최적인 것을 구해주면 된다.

2. [two-pointer 개념](https://butter-shower.tistory.com/226)으로도 풀 수 있다. (O(N))

# 코드

## 첫 번쨰 접근

```javascript
var maxProfit = function (prices) {
	let profit = 0;
	let minPrice = Number.MAX_SAFE_INTEGER;
	for (let i = 0; i < prices.length; i += 1) {
		profit = Math.max(profit, prices[i] - minPrice);
		minPrice = Math.min(minPrice, prices[i]);
	}
	return profit;
};
```

## two-pointer 접근

```javascript
var maxProfit = function (prices) {
	let i = 0;
	let j = i + 1;

	let maxPrice = 0;

	while (j < prices.length) {
		const left = prices[i];
		const right = prices[j];

		if (right < left) i = j;

		j++;

		maxPrice = Math.max(maxPrice, right - left);
	}

	return maxPrice;
};
```
