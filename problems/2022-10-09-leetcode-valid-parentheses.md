---
title: LeetCode, Valid parentheses

date: 2022-10-9

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

길이가 N인 string이 주어진다. string은 문자 '(',')','{','}','\[','\]'로만 이루어져있다. 여는 괄호는 같은 종류의 닫는 괄호와 짝을 이룬다. string이 valid하기 위해서는

1: 서로 다른 괄호끼리 겹쳐서는 안 된다. (괄호를 상자라고 생각하면 된다. 상자 안에 또 다른 상자를 넣을 수는 있지만, 한 개의 물건을 두 개의 상자에 넣을 수는 없다.)

- valid 예시: ()({})\[\]
- not valid 예시: ({)}

2: 괄호는 반드시 짝을 이루어야한다.(열린 괄호, 닫힌 괄호만 남는 경우 not valid이다.)

# 문제접근

1. parentheses나 html tag의 유효성 검사는 stack의 개념을 통해 할 수 있다는 사실을 알고있다. array의 요소를 한 번씩만 조회해서 풀어낼 수 있으므로 O(N) 이다.

# 코드

```javascript
var isValid = function (s) {
	const stack = [];
	const openers = ["(", "{", "["];
	const closers = [")", "}", "]"];
	for (let i = 0; i < s.length; i++) {
		const bracket = s[i];
		if (openers.includes(bracket)) {
			stack.push(bracket);
		} else {
			if (stack.length === 0) return false; // 짝을 이루는지 확인

			// 다른 종류의 괄호와 겹치는지 확인
			if (openers.indexOf(stack[stack.length - 1]) !== closers.indexOf(bracket))
				return false;

			// locally valid
			stack.pop();
		}
	}

	if (stack.length !== 0) return false; // 짝을 이루는지 확인

	return true;
};
```
