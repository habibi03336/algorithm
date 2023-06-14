---
title: LeetCode, 349. Intersection of Two Arrays

date: 2023-1-23

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/intersection-of-two-arrays/description/)

두 배열이 주어진다. 두 배열의 공통된 요소를 중복없이 반환하라.

- 1 <= nums1.length, nums2.length <= 1000
- 0 <= nums1[i], nums2[i] <= 1000

# 문제접근

1. set을 활용하여 문제를 풀어줄 수 있다. O(N)이지만 hash map을 이용하므로 약간의 오버헤드가 있을 수 있다.

2. 숫자의 범위가 그렇게 넓지 않으므로 array를 이용한 counting을 할 수 있다. 중복을 제거하기 위해서, nums1에 있는 경우 1로 업데이트하고, nums2에 있으면 결과에 추가하고 0으로 다시 바꾼다. O(N)

3. sort후 포인터 활용 O(NlogN)

# 코드

## set 활용

```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function (nums1, nums2) {
	const s1 = new Set(nums1);
	const s2 = new Set(nums2);
	return [...s1].filter((elem) => s2.has(elem));
};
```

## array counting 활용

```javascript
var intersection = function (nums1, nums2) {
	const counts = Array(1000 + 1).fill(0);
	for (let i = 0; i < nums1.length; i += 1) {
		counts[nums1[i]] = 1;
	}
	const res = [];
	for (let i = 0; i < nums2.length; i += 1) {
		if (counts[nums2[i]] === 1) {
			res.push(nums2[i]);
			counts[nums2[i]] = 0;
		}
	}
	return res;
};
```

## sort후 포인터 활용

```javascript
var intersection = function (nums1, nums2) {
	nums1.sort((a, b) => a - b);
	nums2.sort((a, b) => a - b);
	const res = [];
	let j = 0;
	for (let i = 0; i < nums1.length; i += 1) {
		if (i >= 1 && nums1[i] === nums1[i - 1]) continue;
		while (j < nums2.length && nums2[j] <= nums1[i]) {
			if (nums2[j] === nums1[i]) {
				res.push(nums1[i]);
				break;
			}
			j += 1;
		}
	}
	return res;
};
```
