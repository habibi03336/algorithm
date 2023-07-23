---
title: LeetCode, Search in Rotated Sorted Array

date: 2022-11-21
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/search-in-rotated-sorted-array/)

rotated sorted array는 오름차순으로 정렬되어있는 array의 특정 index 이후 요소들을 array 앞 쪽에 붙인 것을 의미한다. 다른 말로, 길이가 n인 array가 있을 때 k부터 n-1까지 까지 array 뒤에 0부터 k-1까지의 array를 붙인 것을 의미한다.

> rotated sorted array example
> [1, 2, 3, 4, 5] => [4, 5, 1, 2, 3]  
> [1, 7, 9, 10, 11, 18] => [18, 1, 7, 9, 10, 11]

"possibly" rotated sorted array가 주어지고, array에서 찾아야하는 값인 target이 주어질 때, target 들어있는 index를 반환하라. 알고리즘은 O(logN) 시간복잡도를 가져야만 한다.

- array의 모든 요소는 unique하다.
- 주어지는 array는 rotated sorted array일수도 있고 그냥 오름차순의 array일 수도 있다.("possibly")

# 문제접근

1. 이진탐색을 활용하여 접근했다. 복잡도를 낮추기 위해 두 단계로 나누어 접근했다. 처음에는 이진탐색을 통해 rotate된 위치를 찾는다. 두 번째는 pivot된 위치를 반영하여 target 값을 찾는 일반적인 이진탐색을 한다.

## 1. 첫 번째 이진탐색을 통해서 끊어진 위치를 찾는다.

예를 들어 [4, 5, 1, 2, 3] 에서 rotatedArray2.부터 끊어져 있다는 것을 알 수 있다.

## 2. rotated 되기 전 오름차순 array에서의 pivot index를 구한다.

원래는 [1, 2, 3, 4, 5] 였으니까 originalArray3.에서 pivot 했다는 것을 알 수 있다.

## 3. rotated 되기 전 array를 이진탐색 한다고 가정하고 index를 그에 맞게 변경해준다.

[1, 2, 3]을 A part [4, 5]를 B part라고 하면

만약 pivot 이후에 값을 접근하면 index - (A part 개수) 해줘야한다.

=> originalArray3.을 접근하기 위해서는 rotatedArray\[0\]을 접근해야 한다.

만약 pivot 이전의 값을 접근하면 index + (B part 개수) 해야한다.

=> originalArray\[0\]을 접근하기 위해서는 rotatedArray2.을 접근해야 한다.

# 코드

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function (nums, target) {
	let lo = 0;
	let hi = nums.length - 1;
	let distortedIdx = 0;
	while (lo <= hi && !(lo === nums.length - 1)) {
		const mid = Math.floor((lo + hi) / 2);
		if (nums[mid] > nums[mid + 1]) distortedIdx = mid + 1;
		// mid가 0 일 때 같은 경우가 생기는 데 이때는 lo = mid + 1 을 해줘야 모든 경우를 탐색한다.
		if (nums[mid] >= nums[0]) lo = mid + 1;
		if (nums[mid] < nums[0]) hi = mid - 1;
	}
	const pivot = nums.length - distortedIdx;
	const pivotedIndex = (idx) => {
		return idx < pivot ? idx + distortedIdx : idx - pivot;
	};
	lo = 0;
	hi = nums.length - 1;
	while (lo <= hi) {
		const mid = Math.floor((lo + hi) / 2);
		const midVal = nums[pivotedIndex(mid)];
		if (midVal === target) return pivotedIndex(mid);
		if (midVal < target) lo = mid + 1;
		if (midVal > target) hi = mid - 1;
	}
	return -1;
};
```
