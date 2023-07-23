---
title: LeetCode, 332. Reconstruct ltinerary

date: 2023-1-9
categories:
  - 문제풀이

tags:
  - Exercise
  - 🧑🏻‍💻
---

# [문제요약](https://leetcode.com/problems/reconstruct-itinerary/description/)

여러 장의 항공권이 주어진다. 항공권에는 출발지와 도착지가 명시되어있다. 모든 항공권을 사용하는 여정을 만들 수 있을 때 여정의 경로를 반환한다. 여정의 시작은 'JFK'에서 한다.

- 여러 여정이 있을 수 있을 때는, 가장 사전적으로 앞 서 있는 여정을 반환한다.
- ticket의 개수: [1, 300]

# 문제접근

1. 유향 그래프의 오일러 경로를 구하는 문제이다.

# 코드

```javascript
/**
 * @param {string[][]} tickets
 * @return {string[]}
 */
var findItinerary = function (tickets) {
	tickets.sort();
	const adjMap = {};
	tickets.forEach((ticket) => {
		if (!adjMap[ticket[0]]) adjMap[ticket[0]] = [ticket[1]];
		else adjMap[ticket[0]].push(ticket[1]);
	});
	const res = [];
	const dfs = (node) => {
		while (adjMap[node] && adjMap[node].length !== 0) {
			dfs(adjMap[node].shift());
		}
		res.unshift(node);
	};
	dfs("JFK");
	return res;
};
```
