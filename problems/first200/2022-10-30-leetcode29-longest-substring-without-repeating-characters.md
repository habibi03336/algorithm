---
title: LeetCode, Longest Substring without Repeating Characters

date: 2022-10-30

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

문자열 s가 주어졌을 때, 같은 문자가 나오지 않는 가장 긴 부분 문자열의 길이를 반환하라.

- s의 length는 0 ~ 50,000

# 문제접근

1. Caterpiller 방식을 활용하여 보다 효율적으로 접근할 수 있다. left와 right의 개념으로 right를 하나 씩 오른쪽으로 옮긴다. 그러다가 기존에 있던 문자를 만나면 기존의 문자 다음 문자까지 left를 옮겨준다.

2. 기존에 있던 문자인지 확인해 주기 위해서 Set 객체(집합 개념)을 활용했다.

3. 결과적으로 O(N)의 성능을 기대할 수 있다.

# 코드

```javascript
var lengthOfLongestSubstring = function (s) {
	let maxLen = 0;
	let left = 0;
	const bag = new Set();
	for (let right = 0; right < s.length; right++) {
		if (bag.has(s[right])) {
			while (s[left] !== s[right]) {
				bag.delete(s[left]);
				left++;
			}
			left++;
		}
		bag.add(s[right]);
		maxLen = Math.max(maxLen, right - left + 1);
	}
	return maxLen;
};
```
