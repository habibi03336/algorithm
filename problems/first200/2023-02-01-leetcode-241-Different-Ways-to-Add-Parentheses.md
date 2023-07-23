---
title: LeetCode, 241. Different Ways to Add Parentheses

date: 2023-2-1

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/different-ways-to-add-parentheses/description/)

수식이 들어있는 문자열 expression이 주어진다. 수식을 괄호로 묶는 모든 경우에 대한 결과값을 반환하라.

- 반환 순서는 상관없다.
- expression의 길이의 범위: \[1, 20\]

# 문제접근

1. 분할 정복 개념으로 풀어줄 수 있다. 반복문을 통해 앞 부분의 길이가 1일 때, 2일 때 3일 때... 모든 경우를 만들어 줄 수 있다. 그 안에서 두 부분으로 나누고, 앞 부분에서 가능한 모든 경우의 수 \* 뒤 부분에서 가능한 모든 경우의 수를 해주면되다.

- 문제를 잘 읽는 것이 좋다. 가능한 모든 unique한 결과값을 구하는 것이 아니다. 예제를 충분히 보고 이해한 후 푸는 것이 좋은 방향 같다.

# 코드

## 정리된 코드

```javascript
/**
 * @param {string} expression
 * @return {number[]}
 */

const calculate = (a, op, b) => {
	if (op === "*") return a * b;
	if (op === "+") return a + b;
	if (op === "-") return a - b;
};

const operators = ["*", "-", "+"];
var diffWaysToCompute = function (expression) {
	// simple solution
	if (!isNaN(expression)) return [Number(expression)];

	// devide & conquer
	const res = [];
	for (let i = 0; i < expression.length; i += 1) {
		if (isNaN(expression[i])) {
			const a = diffWaysToCompute(expression.slice(0, i));
			const b = diffWaysToCompute(expression.slice(i + 1));
			for (let num1 of a) {
				for (let num2 of b) {
					res.push(calculate(num1, expression[i], num2));
				}
			}
		}
	}
	return res;
};
```

## 초기 코드

```javascript
/**
 * @param {string} expression
 * @return {number[]}
 */

const calculate = (a, op, b) => {
	if (op === "*") return a * b;
	if (op === "+") return a + b;
	if (op === "-") return a - b;
};

const operators = ["*", "-", "+"];
var diffWaysToCompute = function (expression) {
	// parsing
	let parsed = [];
	let prev = 0;
	let i = 0;
	while (i < expression.length) {
		if (operators.includes(expression[i])) {
			parsed.push({ type: "num", num: Number(expression.slice(prev, i)) });
			parsed.push({ type: "operator", idx: i });
			prev = i + 1;
		}
		i += 1;
	}
	parsed.push({ type: "num", num: Number(expression.slice(prev)) });

	// simple solution
	if (parsed.length === 1) return [parsed[0].num];

	// devide & conquer
	const res = [];
	for (let i = 0; i < parsed.length; i += 1) {
		if (parsed[i].type === "operator") {
			const a = diffWaysToCompute(expression.slice(0, parsed[i].idx));
			const b = diffWaysToCompute(expression.slice(parsed[i].idx + 1));
			for (let num1 of a) {
				for (let num2 of b) {
					res.push(calculate(num1, expression[parsed[i].idx], num2));
				}
			}
		}
	}
	return res;
};
```
