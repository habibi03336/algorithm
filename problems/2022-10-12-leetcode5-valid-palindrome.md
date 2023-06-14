---
title: LeetCode, Valid Palindrome

date: 2022-10-12

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

알파벳과 숫자를 제외한 문자를 제외하고 알파벳을 모두 소문자로 바꾸었을 때, 앞에서부터 읽는 것과 뒤에서 부터 읽는 것이 같으면 이를 'parlindrome'이라고 한다. 길이가 N인 string이 주어질 때, 해당 문자열이 palindrome인지 확인하라.

- N은 200,000
- string의 문자는 ascii 문자로만 이루어져 있다.

# 문제접근

1. 알파벳과 숫자만 뽑아준 다음에, 투 포인터로 앞 쪽과 뒷 쪽을 차례로 비교해 주면 된다.

# 코드

Runtime: 122ms

```javascript
var isPalindrome = function (s) {
	const filteredString = [...s]
		.map((elem) => {
			const ascii = Number(elem.charCodeAt(0));

			if ((ascii >= 48 && ascii <= 57) || (ascii >= 97 && ascii <= 122))
				return elem;

			if (ascii >= 65 && ascii <= 90) {
				return String.fromCharCode(ascii + 32);
			}

			return "";
		})
		.join("");

	let i = 0;
	let j = filteredString.length - 1;
	while (i < j) {
		if (filteredString[i] !== filteredString[j]) return false;
		i++;
		j--;
	}

	return true;
};
```

## 가장 성능이 좋은 풀이

내 풀이에서 전개연산자를 써서 filtering하고 다시 join하는 거 때문에 성능이 다소 떨어진 것 같다. String 객체의 replace, toLowerCase 메쏘드를 쓰면 훨씬 빠르다.
Runtime: 62 ms

```javascript
const alphaNumOnly = (str) => {
	const alphaNum = str.replace(/[^a-z0-9]/gi, "");
	return alphaNum.toLowerCase();
};

/**
 * @param {string} s
 * @return {boolean}
 */
var isPalindrome = function (s) {
	let str = alphaNumOnly(s);
	for (let i = 0, j = str.length - 1; i < j; i++, j--) {
		if (str[i] !== str[j]) {
			return false;
		}
	}

	return true;
};
```
