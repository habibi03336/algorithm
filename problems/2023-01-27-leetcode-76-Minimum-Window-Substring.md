---
title: LeetCode, 76. Minimum Window Substring

date: 2023-1-27

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/minimum-window-substring/description/)

두 문자열 s와 t가 주어진다. 중복을 포함하여 t의 모든 문자를 가지고 있는 s의 가장 짧은 부분 문자열을 반환하라. 답이 없으면 빈 문자열을 반환하라.

- 정답이 unique하게만 입력값이 주어진다.
- s와 t는 알파벳 대문자, 소문자로만 이루어져있다.
- s.length(m), t.length(n)의 범위: \[1, 100,000\]

# 문제접근

1. s의 문자 중에 t에 속하는 문자를 시작점으로 이후에 최대한 짧게 조건을 만족하는 경우 알아낸다. 여러 시작점을 모두 고려하여 가장 짧은 답을 구해줄 수 있다. 최악의 경우 O(n^2) 일 수 있다. 타임아웃이 났다.

- 최악의 케이스: s = "AAAAAAAAA.....C", t="AC"

2. counter와 caterpillar 방식으로 O(n)으로 풀어 줄 수 있다.

# 코드

## 각 시작점에 대해서 모두 고려해주는 경우 (타임아웃)

```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {string}
 */
var minWindow = function (s, t) {
	const setT = new Set(t);
	const words = [];
	for (let i = 0; i < s.length; i += 1) {
		if (setT.has(s[i])) {
			words.push(i);
		}
	}
	const defaultCounter = {};
	for (let i = 0; i < t.length; i += 1) {
		if (defaultCounter[t[i]] === undefined) {
			defaultCounter[t[i]] = 1;
		} else {
			defaultCounter[t[i]] += 1;
		}
	}
	let resIndex = [0, Number.MAX_SAFE_INTEGER];
	for (let i = 0; i < words.length; i += 1) {
		const counter = { ...defaultCounter };
		let missing = t.length;
		let j = i;
		while (j < words.length) {
			if (counter[s[words[j]]] > 0) {
				counter[s[words[j]]] -= 1;
				missing -= 1;
			}
			if (resIndex[1] - resIndex[0] < words[j] - words[i]) {
				break;
			} else if (missing === 0) {
				resIndex = [words[i], words[j]];
			}
			j += 1;
		}
	}
	return resIndex[1] === Number.MAX_SAFE_INTEGER
		? ""
		: s.slice(resIndex[0], resIndex[1] + 1);
};
```

## caterpillar 방식

```javascript
var minWindow = function (s, t) {
	const counter = {};
	for (let i = 0; i < s.length; i += 1) {
		counter[s[i]] = 0;
	}
	for (let i = 0; i < t.length; i += 1) {
		if (counter[t[i]] === undefined) counter[t[i]] = 1;
		else counter[t[i]] += 1;
	}
	let start = 0;
	let end = Number.MAX_SAFE_INTEGER;
	let left = 0;
	let missing = t.length;
	for (let right = 0; right < s.length; right += 1) {
		missing -= counter[s[right]] > 0;
		counter[s[right]] -= 1;

		if (missing === 0) {
			while (left <= right && counter[s[left]] < 0) {
				counter[s[left]] += 1;
				left += 1;
			}
			if (end - start > right - left) {
				end = right;
				start = left;
			}
			counter[s[left]] += 1;
			missing += 1;
			left += 1;
		}
	}
	return end === Number.MAX_SAFE_INTEGER ? "" : s.slice(start, end + 1);
};
```
