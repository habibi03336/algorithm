---
title: LeetCode, 316. Remove Duplicate Letters

date: 2023-1-18

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/remove-duplicate-letters/description/)

주어진 길이가 n인 문자열 s에서, 중복되는 문자를 제거하라. 중복된 문자를 제거하는 모든 경우의 수 가운데 사전적 순서에서 가장 작은 경우을 반환하라.

- s는 영어 소문자로만 이루어져있다.
- n의 범위: [1, 10,000]

# 문제접근

1. count와 stack을 활용하여서 문제를 접근할 수 있다. 문자를 stack에 넣어놓고 조건에 따라서 pop해주는 방식으로 접근할 수 있다.

- stack에 새로 넣으려는 문자에 대해서, stack에 이미 있는 문자가 사전적으로 더 큰 문자이고, 뒤에도 또 나올 것이라면 pop되고 뒤에서 다시 들어오는 것이 맞다.

# 코드

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var removeDuplicateLetters = function (s) {
	const counts = Array(26).fill(0);
	const offset = "a".charCodeAt(0);
	for (let i = 0; i < s.length; i++) {
		counts[s.charCodeAt(i) - offset] += 1;
	}
	const stack = [];
	const seen = new Set();
	for (let i = 0; i < s.length; i++) {
		const charCode = s.charCodeAt(i);
		counts[charCode - offset] -= 1;
		if (seen.has(charCode)) continue;
		while (stack.length !== 0) {
			const stackTop = stack.pop();
			seen.delete(stackTop);
			if (stackTop < charCode || counts[stackTop - offset] === 0) {
				stack.push(stackTop);
				seen.add(stackTop);
				break;
			}
		}
		stack.push(charCode);
		seen.add(charCode);
	}
	return String.fromCharCode(...stack);
};
```
