---
title: LeetCode, String to Integer (atoi)

date: 2022-12-9
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/string-to-integer-atoi/)

문자열 s를 받아서 32bit integer를 반환하는 함수를 구현하라.

1. 앞 쪽에 있는 white space는 모두 무시한다.
2. 숫자 시작 하기 앞서서 \- or + 가 있는지 확인한다. 이는 정수의 부호를 결정한다.
3. 숫자가 아닌 문자가 나올 때까지 읽는다. 숫자가 아닌 문자열이 나오면 그 뒤는 무시한다.
4. 아무런 숫자도 없으면 0 이다.
5. 32 bit integer의 범위는 [-2\*\*31, 2\*\*31-1] 이다. 범위에 벗어나는 값이라면 -2\*\*31이나 2\*\*31-1로 clamp 한다.

- 0 <= s.length <= 200
- javascript는 32bit integer 원시타입이 없고 부동소수점 타입만 있기 때문에 clamp할 때 손쉽게 Math.max 등을 활용할 수도 있다. 하지만 그렇게 하지 않고 32bit integer 타입만 있다고 가정하고 문제에 접근했다.

# 문제접근

1. stack에 digit을 넣은 후에 자리 수에 맞게 연산하여 구할 수 있다.

2. 여러가지 경우의 수에 대해서 생각해줘야 하는 문제이다. startRead flag를 통해서 숫자를 읽기 시작한 경우가 아직 시작하지 않은 경우를 나누어서 봐주었다.

- 아직 숫자를 읽지 않았고, 이번 문자도 숫자가 아닌 경우

  1.  문자가 부호인 경우 => 부호를 저장하고 숫자 읽기 시작
  2.  문자가 whitespace인 경우 => 무시
  3.  부호도 아니고 whitespace도 아닌 경우 => 읽기

- 숫자를 읽기 시작했는데 non-digit 문자인 경우 => 읽기 끝냄

- digit 문자인 경우 => 숫자를 계속 읽음

3. 각 자리 수를 비교하여서 clamp를 할지 하지 않을지 결졍해주었다. 높은 자리에서 크고 작은게 명확히 결정되면 clamp를 할 지 안할지 바로 결정할 수 있다. 만약 값이 같다면 그 아래 자리를 순차적으로 비교해주면 된다.

4. s 문자열을 한 번 씩 조회하는게 가장 큰 시간복잡도를 가지기 때문에 O(N)이다.

# 구현 방식

1. starRead flag는 idempotent한 방식으로, 숫자를 읽을 때, 또는 부호를 만났을 때 true를 계속 할당하도록 했다.

2. for문과 break를 활용하여 각 자리수 비교하고, 전체 크기에 대한 대소 판단을 해주었다.

# 코드

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var myAtoi = function (s) {
	const stack = [];
	let startRead = false;
	let isPositive = true;
	//48 - 57
	for (let i = 0; i < s.length; i++) {
		const code = s.charCodeAt(i);
		const isDigit = code >= 48 && code <= 57;
		if (!startRead && !isDigit) {
			if (code === 43 || code == 45) {
				isPositive = code === 43;
				startRead = true;
				continue;
			}
			if (code === 32) continue;
			break;
		}
		if (startRead && !isDigit) {
			break;
		}
		if (isDigit) {
			startRead = true;
			stack.push(s[i]);
			continue;
		}
	}
	while (stack.length > 0) {
		if (stack[0] === "0") stack.shift();
		else break;
	}

	let integer = 0;
	let i = 0;
	let overRange = false;
	if (stack.length > 10) {
		overRange = true;
	}
	const max_integers = String(2147483647).split("");
	if (stack.length === 10) {
		for (let i = 0; i < 10; i++) {
			if (stack[i] > max_integers[i]) {
				overRange = true;
				break;
			} else if (stack[i] < max_integers[i]) {
				break;
			}
		}
	}
	if (overRange) {
		if (isPositive) return 2 ** 31 - 1;
		if (!isPositive) return -1 * 2 ** 31;
	}
	while (stack.length > 0) {
		integer += 10 ** i * Number(stack.pop());
		i++;
	}
	if (!isPositive) integer *= -1;
	return integer;
};
```
