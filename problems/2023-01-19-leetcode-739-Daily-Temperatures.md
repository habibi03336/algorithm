---
title: LeetCode, 739. Daily Temperatures

date: 2023-1-19

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/remove-duplicate-letters/description/)

해당 날의 온도를 담은 길이가 n인 배열 T가 주어진다. 각 날짜에서 더 따뜻한 날이 될 때까지 얼마나 기다려야하는지 배열로 반환하라.

- 더 따뜻한 날을 찾지 못하면 0을 넣어 반환한다.
- n은 [1, 100,000]

# 문제접근

1. 브루트 포스로 접근하면 O(n^2)이라서, 조건에 따르면 100억 번 수준의 연산이 있을 수 있다. 따라서 브루트포스는 불가능하다.

2. stack을 활용하여 O(n)으로 풀 수 있다. stack에 인덱스를 쌓아놓고, 더 높은 온도를 만나는 경우 pop하여 인덱스 끼리의 차를 답으로 넣어주면 된다.

- stack에는 맨 아래부터 위쪽으로, 온도가 내림차순으로 들어갈 수밖에 없다. 그렇지 않으면 앞서서 이미 pop되었을 것이기 때문이다.

# 코드

```javascript
/**
 * @param {number[]} temperatures
 * @return {number[]}
 */
var dailyTemperatures = function (temperatures) {
	const answer = Array(temperatures.length).fill(0);
	const stack = [];
	for (let i = 0; i < temperatures.length; i++) {
		while (
			stack.length !== 0 &&
			temperatures[stack[stack.length - 1]] < temperatures[i]
		) {
			const idx = stack.pop();
			answer[idx] = i - idx;
		}
		stack.push(i);
	}
	return answer;
};
```
