---
title: LeetCode, First Bad Version

date: 2022-10-21

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/first-bad-version/)

1부터 n까지의 버전으로 개발된 프로덕트가 있다. 중간에 한 버전이 나쁘게 개발되었을 경우 그 이후 버전도 모두 나쁘게 개발된다. 이 프로덕트가 나쁘게 개발되었다고 할 때 처음으로 나쁘게 개발된 버전을 찾아라.

- n은 1부터 2^31 - 1 까지
- bad version 인지 확인은 isBadVersion API를 통해서 알 수 있고 해당 호출을 최소화 하라.

# 문제접근

1. 하단과 상단이 정해져있고, 답의 도메인이 정수(유한 집합)이고, 선형적인 조건을 가지기 때문에 이진탐색을 활용하여 풀 수 있다고 생각했다. 이진탐색 기본 탬플릿으로 코드를 작성했다.

2. 마지막 좋은 버전을 찾아내는 이진탐색 코드를 짰다. 좋은 버전이라면 바로 다음 버전이 나쁜 것인지 확인하는 방식으로 마지막 좋은 버전을 찾아낼 수 있다. 이 때 주의해야할 것은 만약 가장 처음부터 나쁜 버전이었다면 마지막 좋은 버전이 없으므로 엣지 케이스가 생긴다.

# 코드

```javascript
/**
 * Definition for isBadVersion()
 *
 * @param {integer} version number
 * @return {boolean} whether the version is bad
 * isBadVersion = function(version) {
 *     ...
 * };
 */

/**
 * @param {function} isBadVersion()
 * @return {function}
 */
var solution = function (isBadVersion) {
	/**
	 * @param {integer} n Total versions
	 * @return {integer} The first bad version
	 */
	return function (n) {
		// 엣지 케이스 없애주기
		if (isBadVersion(1)) return 1;

		let lo = 1;
		let hi = n;
		while (lo <= hi) {
			const mid = Math.floor((lo + hi) / 2);
			const isBad = isBadVersion(mid);
			if (isBad) {
				hi = mid - 1;
			} else {
				const isNextBad = isBadVersion(mid + 1);
				if (isNextBad) return mid + 1;
				lo = mid + 1;
			}
		}
	};
};
```
