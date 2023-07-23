---
title: LeetCode, Longest Palindrome

date: 2022-10-23
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/longest-palindrome/submissions/)

알파벳을 앞에서부터 읽으나 뒤에서부터 읽으나 똑같이 읽히는 문자열을 palindrome이라고 한다. 어떤 문자열 s가 주어졌을 때 해당 문자열의 문자로 만들 수 있는 가장 긴 palindrome의 길이를 반환하라.

- s.length 1 ~ 2000
- s는 영어 알파벳으로만 이루어져있고 case sensitive하다.

# 문제접근

1. ascii와 array index를 바인딩하여 문자를 카운트를 기록해 주었다. 문자의 개수 이하의 최대 2의 배수 만큼 palindrome에 추가할 수 있다.(7이면 6, 8이면 8, 11이면 10) 또한 palindrome의 가운데 있는 문자는 하나만 있어도 된다. 따라서 가운에 하나로 들어갈 문자가 있는지 확인해주어야 한다.(홀수 개인 문자가 있는지 확인한다.)

# 코드

```javascript
var longestPalindrome = function (s) {
	const wordsCount = Array(58).fill(0);
	for (let i = 0; i < s.length; i++) {
		const ascii = s.charCodeAt(i);
		wordsCount[ascii - 65] += 1;
	}
	let count = 0;
	let alone = 0;
	wordsCount.forEach((elem) => {
		if (elem === 0) return;
		if (elem === 1) {
			alone = 1;
			return;
		}
		if (elem % 2 === 1) alone = 1;
		count += Math.floor(elem / 2) * 2;
	});

	return count + alone;
};
```
