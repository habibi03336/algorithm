---
title: LeetCode, Evaluate Reverse Polish Notation

date: 2022-11-4
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

[Reverse Polish Notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation)이 array 주어졌을 때 계산 결과를 반환하라.

- array의 length 는 1 ~ 10,000

# 문제접근

1. token array를 앞 쪽부터 조회하면서 stack에 숫자와 계산 결과(operands)를 쌓고, operator가 나올 경우 stack에서 right operand와 left operand를 순서대로 pop하여 계산해주면 된다.

- 처음에 result 변수를 따로 만들어서 결과값을 만들어가려고 했었는데 그런 방식으로 접근하니, 계산 순서를 맞춰주기가 복잡해져서 헤맸다. 그래서 구현을 하는데 시간이 좀 들었다.

- 계산 결과를 다시 stack에 넣어주면 순서에 맞게 잘 계산을 한다.

# 코드

```javascript
/**
 * @param {string[]} tokens
 * @return {number}
 */

const operators = ["+", "-", "*", "/"];

const stringCalculator = (stringNum, operator, stringNum2) => {
	const num = Number(stringNum);
	const num2 = Number(stringNum2);

	if (operator === "+") return num + num2;
	if (operator === "-") return num - num2;
	if (operator === "/") return Math.trunc(num / num2);
	if (operator === "*") return num * num2;
};

var evalRPN = function (tokens) {
	const tokenLen = tokens.length;
	const numStack = [];
	for (let i = 0; i < tokenLen; i++) {
		const token = tokens[i];
		if (operators.includes(token)) {
			const right = numStack.pop();
			const left = numStack.pop();
			const subResult = stringCalculator(left, token, right);
			numStack.push(subResult);
		} else {
			numStack.push(token);
		}
	}

	return numStack[0];
};
```
