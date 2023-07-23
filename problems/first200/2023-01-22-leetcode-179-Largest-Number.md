---
title: LeetCode, 179. Largest Number

date: 2023-1-22

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/largest-number/description/)

0 이상의 정수가 담긴 배열의 요소를 문자처럼 이어붙여서 만들 수 있는 가장 큰 수를 반환하라.

# 문제접근

1. 비교 함수를 문제 조건에 맞도록 재정의 해주는 방식으로 풀 수 있다.

2. 비교 함수는 a,b 두 요소가 주어질 때, 반환 값이 음수이면 a가 b보다 앞에, 양수이면 b가 a 앞에 있어야 함을 의미한다. 즉 ab, ba가 있을 때, ab가 더 크다면 음수, ba가 더 크다면 양수를 반환해야한다. 이는 ba - ab로 나타낼 수 있다.

# 코드

```javascript
/**
 * @param {number[]} nums
 * @return {string}
 */
var largestNumber = function (nums) {
	const compareFn = (a, b) => {
		const ab = Number([a, b].join(""));
		const ba = Number([b, a].join(""));
		return ba - ab;
	};
	nums.sort(compareFn);
	return nums[0] === 0 ? "0" : nums.join("");
};
```
