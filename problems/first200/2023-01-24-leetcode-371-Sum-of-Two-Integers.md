---
title: LeetCode, 371. Sum of Two Integers

date: 2023-1-25

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/sum-of-two-integers/description/)

두 정수의 합을 + 나 - 연산 없이 구하여라.

- 정수의 범위: [-1,000, 1000]

# 문제접근

1. 비트 연산으로 구해줄 수 있다. AND 연산으로 올려야할 자리를 알 수 있고, XOR 연산으로 한쪽에만 1이 있는 비트를 알 수 있다. 더 이상 올려야 할 것이 없을 때까지 반복하다가 OR 연산을 해주면 된다.

# 코드

```javascript
/**
 * @param {number} a
 * @param {number} b
 * @return {number}
 */
var getSum = function (a, b) {
	let bits = a ^ b;
	let carry = a & b;
	carry = carry << 1;
	while ((carry & bits) !== 0) {
		[bits, carry] = [carry ^ bits, carry & bits];
		carry = carry << 1;
	}
	return carry | bits;
};
```
