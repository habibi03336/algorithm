---
title: LeetCode, 239. Sliding Window Maximum

date: 2023-1-27

categories:
  - 문제풀이

tags:
  - Exercise
  - 🧑🏻‍💻
---

# [문제요약](https://leetcode.com/problems/longest-repeating-character-replacement/description/)

대문자 알파벳으로 이루어진 문자열 s가 주어진다. s의 문자 중 아무것이나 골라 원하는 문자로 k번 바꿀 수 있다. 이 때 연속하여 같은 문자가 나오는 가장 긴 길이를 반환하라.

# 문제접근

1. caterpillar 방식으로 풀이할 수 있다. 오른쪽 포인터를 오른쪽 한 칸씩 움직인다. 그러다가 어떤 문자를 기준으로해도 "k번 바꿀 시 연속"이라는 조건을 만족하지 못하면 왼쪽 포인터를 움직인다.

# 코드

```javascript
/**
 * @param {string} s
 * @param {number} k
 * @return {number}
 */
var characterReplacement = function (s, k) {
	const offset = "A".charCodeAt(0);
	const counter = Array(26).fill(0);
	let maxCount = 0;
	let cnt = 0;
	let left = 0;
	for (let i = 0; i < s.length; i += 1) {
		counter[s.charCodeAt(i) - offset] += 1;
		cnt += 1;
		while (counter.every((elem) => cnt - elem > k)) {
			counter[s.charCodeAt(left) - offset] -= 1;
			cnt -= 1;
			left += 1;
		}
		maxCount = Math.max(maxCount, cnt);
	}
	return maxCount;
};
```
