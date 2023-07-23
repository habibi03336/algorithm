---
title: LeetCode, 455. Assign Cookies

date: 2023-1-31

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/assign-cookies/description/)

여러 명의 아이들이 있다. 각 아이들의 만족 수준을 담은 배열 g가 주어진다. 또 쿠키의 만족감을 담은 배열 s가 주어진다. 쿠키의 만족감이 아이의 만족 수준보다 크거나 같을 때(g\[i\] >= s\[j\]) 아이는 만족한다.

아이에게 딱 하나의 쿠키만을 줄 수 있다고 할 때, 만족하는 아이의 수를 최대화화고 그 수를 반환하라.

# 문제접근

1. 만족하는 아이의 수를 최대화 해야한다. 가장 만족 수준이 낮은 아이부터 최대한 작은 쿠키를 주면서 만족시키면 조건을 만족한다.(그리디한 접근)

# 구현

1.

1)만족감이 작은 아이부터

2)최대한 작은 쿠키를 준다

이를 쉽게 다루기 위해서 g와 s를 정렬하고 반복문을 통해서 카운팅해준다.

# 코드

```javascript
/**
 * @param {number[]} g
 * @param {number[]} s
 * @return {number}
 */
var findContentChildren = function (g, s) {
	g.sort((a, b) => a - b);
	s.sort((a, b) => a - b);
	let cnt = 0;
	let i = 0;
	let j = 0;
	while (i < g.length && j < s.length) {
		if (g[i] <= s[j]) {
			cnt += 1;
			i += 1;
		}
		j += 1;
	}
	return cnt;
};
```
