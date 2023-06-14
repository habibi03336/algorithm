---
title: LeetCode, Valid Anagram

date: 2022-10-13

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

한 문자열의 모든 문자를 한 번씩 사용하여 다른 문자열을 만들 수 있으면 이를 "anagram"이라고 한다. 두 문자열 s, t가 주어질 때 이 둘이 anagram인지 확인하라.

- s, t의 길이는 50,000
- 문자열의 모든 문자는 소문자 영어 알파벳이다.

# 문제접근

1. character는 unicode나 ascii 같은 규칙을 통해 int로 인코딩될 수 있다. 이것과 array의 index를 활용해 각 character가 몇 번 나오는지 저장할 수 있다. 한 문자열에서 나오는 문자를 저장해준 뒤 다른 문자열도 같은 문자를 가지고 있는지 확인해주면 된다.

2. 순서는 다르지만 정확히 같은 요소를 가지고 있어야 한다. 정렬을 활용해 일정한 기준으로 문자의 순서를 맞춘 다음 정확히 같은지 확인해주면 된다.

# 코드

## counting을 이용한 풀이

```javascript
var isAnagram = function (s, t) {
	const charCounts = Array(26).fill(0);
	for (let i = 0; i < s.length; i++) {
		charCounts[s[i].charCodeAt(0) - 97] += 1;
	}

	for (let i = 0; i < t.length; i++) {
		const charIdx = t[i].charCodeAt(0) - 97;
		if (charCounts[charIdx] === 0) return false;
		charCounts[charIdx]--;
	}

	if (charCounts.reduce((pre, cur) => pre + cur, 0) !== 0) return false;

	return true;
};
```

## 정렬을 활용한 풀이

```javascript
var isAnagram = function (s, t) {
	const ss = [...s];
	ss.sort();
	const tt = [...t];
	tt.sort();
	return ss.join("") === tt.join("");
};
```
