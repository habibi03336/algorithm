---
title: Codility, Max Profit

date: 2022-9-2

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

array에 주가가 시간 순으로 담겨있을 때, 주식을 사고 팔아서 낼 수 있는 최대 이익은 얼마였는가를 구하여라. 이익을 내지 못한다면 0을 반환한다.

# 문제접근

1. N이 400,000이였기 때문에 O(N)으로 풀어야 했다. 파는 시점보다 앞선 시점에 언제든 주식을 가장 싸게 사면 최대 이익을 낼 수 있다. 그리고 각 파는 시점의 이익을 비교하면 전체의 최대 이익을 구할 수 있다.

# [코드](https://app.codility.com/demo/results/training98MSBX-QBH/)

```javascript
function solution(A) {
	let minPrice = Number.MAX_VALUE;
	const profits = Array(A.length).fill(0);

	// 각 파는 시점에 낼 수 있는 최대 이익 구하기
	A.forEach((elem, idx) => {
		if (elem < minPrice) minPrice = elem;
		profits[idx] = elem - minPrice;
	});

	// 파는 시점별 이익 중 최대 이익 뽑아내기
	return Math.max(0, ...profits)
}
```
