---
title: LeetCode, Sort Colors

date: 2022-12-1
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/sort-colors/)

길이가 N인 배열이 주어진다. 배열에는 빨간색, 흰색, 파란색을 나타내는 0, 1, 2 값이 들어있다. 빨간색 이후 흰색, 그 이후 파란색 요소들을 가지도록 배열을 정렬하라. 단, 정렬은 inplace 정렬을 해야한다.(extra 공간복잡도가 O(N) 미만이여야 한다.)

```javascript
// Example

// Input:
[2, 0, 2, 1, 1, 0];
// Output:
[0, 0, 1, 1, 2, 2];
```

# 문제접근

1. inplace sort의 대표격인 quick sort로 문제를 해결할 수 있다. O(NlogN)

2. inplace sort의 다른 방식인 insertion sort로 문제를 해결할 수 있다. O(NlogN)

3. 포인터를 활용해서 caterpillar 방식으로 해결할 수 있다. O(N)

\* quick sort에서 pivot을 기준으로 정렬하고, pivot index를 반환할 때에도 caterpillar 방식을 쓴다.

4. counting으로 문제를 해결할 수 있다. O(N)

# 코드

## quick sort

124 ms 42.2 MB

```javascript
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var sortColors = function (nums) {
	const setPivot = (start, end, nums) => {
		//caterpillar 방식
		const pivot = nums[end];
		let pivotIdx = start;
		for (let i = start; i < end; i++) {
			if (nums[i] <= pivot) {
				const tmp = nums[i];
				nums[i] = nums[pivotIdx];
				nums[pivotIdx] = tmp;
				pivotIdx++;
			}
		}
		nums[end] = nums[pivotIdx];
		nums[pivotIdx] = pivot;
		return pivotIdx;
	};
	const quickSort = (start, end, nums) => {
		if (start >= end) return;
		const pivotIdx = setPivot(start, end, nums);
		quickSort(start, pivotIdx - 1, nums);
		quickSort(pivotIdx + 1, end, nums);
	};
	quickSort(0, nums.length - 1, nums);
};
```

## insertion sort

63 ms, 41.4 MB

```javascript
var sortColors = function (nums) {
	let target = 0;
	let cur = target - 1;
	while (target !== nums.length) {
		const tmp = nums[target];
		while (
			cur >= 0 &&
			((nums[cur] === 2 && (tmp === 0 || tmp === 1)) ||
				(nums[cur] === 1 && tmp === 0))
		) {
			nums[cur + 1] = nums[cur];
			cur -= 1;
		}
		nums[cur + 1] = tmp;
		target += 1;
		cur = target - 1;
	}
};
```

## caterpillar

100 ms 42.4 MB

```javascript
var sortColors = function (nums) {
	const RED = 0;
	const BLUE = 2;
	let redIndex = 0;
	let blueIndex = nums.length - 1;
	for (let i = 0; i < nums.length; i++) {
		if (nums[i] === RED) {
			const tmp = nums[redIndex];
			nums[redIndex] = RED;
			nums[i] = tmp;
			redIndex++;
		}
	}
	for (let i = 0; i < nums.length; i++) {
		if (nums[nums.length - 1 - i] === BLUE) {
			const tmp = nums[blueIndex];
			nums[blueIndex] = BLUE;
			nums[nums.length - 1 - i] = tmp;
			blueIndex--;
		}
	}
	return nums;
};
```

## counting

85 ms 41.9 MB

```javascript
var sortColors = function (nums) {
	let redCount = 0;
	let whiteCount = 0;
	let blueCount = 0;
	nums.forEach((elem) => {
		if (elem === 0) redCount++;
		if (elem === 1) whiteCount++;
		if (elem === 2) blueCount++;
	});
	for (let i = 0; i < nums.length; i++) {
		if (0 < redCount--) {
			nums[i] = 0;
		} else if (0 < whiteCount--) {
			nums[i] = 1;
		} else if (0 < blueCount--) {
			nums[i] = 2;
		}
	}
	return nums;
};
```
