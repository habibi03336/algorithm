---
title: LeetCode, 561. Array Partition

date: 2023-1-16

categories:
  - 문제풀이

tags:
  - Exercise
  - 🧑🏻‍💻
---

# [문제요약](https://leetcode.com/problems/array-partition/description/)

정수가 담겨있는 길이 2n의 배열이 주어진다. 배열의 요소를 n개의 쌍으로 묶어서 둘 중 작은 값을 취한 후(min(a,b)) 모두 더한다. 이 때 가능한 가장 큰 총합을 반환하라.

- n의 범위: [1, 10,000]

# 문제접근

1. 두 요소를 쌍으로 묶을 때, 가장 가까운 값끼리 묶으면 총합이 최대가 된다. 따라서 정렬을 한 후, 0부터 짝수번 째 요소를 모두 더해주면 된다.

# 코드

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var arrayPairSum = function (nums) {
	nums.sort((a, b) => a - b);
	return nums.reduce((pre, cur, idx) => {
		if (idx % 2 === 0) {
			pre += cur;
		}
		return pre;
	}, 0);
};
```
