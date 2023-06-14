---
title: LeetCode, Coin Change

date: 2022-11-9
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/coin-change/)

동전의 단위(coins)와 바꿔야 하는 금액(amount)이 주어진다. 가장 적은 동전의 수로 교환을 한다고 할 때 몇 개의 동전을 써야 하는가?

- 단위의 개수는 [1, 12]
- 코인 단위는 [1, 2^31-1]
- 금액은 [0, 10,000]
- 교환이 불가능 하면 -1을 반환한다.

# 문제접근

1. [DP를 활용하여 푸는 대표적인 문제였다.](https://codility.com/media/train/15-DynamicProgramming.pdf)

2. 예를 들어 {1, 3, 4} 동전이 주어지고 6을 만든다고 하자. 1, 3, 4로 6을 만드는 최적은 min(A, B)이다.

- A: 1, 3 단위로만 6을 만들 때 필요한 동전의 개수 최적
- B: 1, 3, 4 단위로 2를 만들 때 필요한 동전의 개수 최적 + 1

dp에서 중요한 것은 간단한 상황에 대해서 구한 값을 통해 더 복잡한 상황의 정답을 찾는 다는 것이다. 이 경우에도 1, 3, 4로 6을 만드는 최적을 더 간단한 상황의 두 값을 비교하여 구한다. 두 개의 축을 활용하는데, 동전 단위 축과 금액 축이다. 동전의 단위가 더 적은 경우, 만들고자 하는 금액이 더 작은 경우 이 두 가지 더 간단한 상황을 통해 솔루션을 얻을 수 있다. 이 과정은 recursive하게 일어난다. 즉 제일 간단한 사실에서 부터 시작하여 조금 더 복잡한 사실의 문제를 해결하고, 그리고 그 조금 더 복잡한 사실의 솔루션을 통해 더 복잡한 상황의 솔루션을 얻어 낸다.

# 코드

```javascript
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function (coins, amount) {
	const dp = Array(amount + 1).fill(Number.MAX_SAFE_INTEGER);
	dp[0] = 0;
	for (let i = 0; i < coins.length; i++) {
		for (let j = coins[i]; j < amount + 1; j++) {
			dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);
		}
	}
	return dp[amount] === Number.MAX_SAFE_INTEGER ? -1 : dp[amount];
};
```
