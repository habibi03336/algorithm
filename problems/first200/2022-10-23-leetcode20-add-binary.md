---
title: LeetCode, Majority Element

date: 2022-10-23
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/add-binary/)

이진수 문자열 두 개가 주어진다. 두 이진수를 더하여 문자열로 반환하라.

- 문자열의 길이는 1 ~ 10,000

# 문제접근

1. 각 자리 수를 작은 자리 수부터 더하여 if문으로 결과값을 만들어 나갈 수 있다.

# 코드

```javascript
var addBinary = function (a, b) {
	if (a === b && a === "0") return "0";

	const result = [];
	let upwardNum = 0;
	let i = 0;
	while (i < a.length || i < b.length) {
		const num1 = a.length > i ? Number(a[a.length - 1 - i]) : 0;
		const num2 = b.length > i ? Number(b[b.length - 1 - i]) : 0;
		const localSum = num1 + num2 + upwardNum;
		if (localSum === 3) {
			result.unshift("1");
			upwardNum = 1;
		} else if (localSum === 2) {
			result.unshift("0");
			upwardNum = 1;
		} else if (localSum === 1) {
			result.unshift("1");
			upwardNum = 0;
		} else if (localSum === 0) {
			result.unshift("0");
			upwardNum = 0;
		}

		i++;
	}

	if (upwardNum === 1) result.unshift("1");
	return result.join("");
};
```
