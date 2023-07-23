---
title: LeetCode, Product of Array Except Self

date: 2022-11-11
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/product-of-array-except-self/)

길이가 N인 정수 array가 주어진다. 각 array index에 대해 자기 자신을 제외한 다른 모든 요소의 곱한 결과를 array로 반환하라.

- 곱셈의 결과는 32bit integer로 표현할 수 있다.
- O(N)의 시간 복잡도와 O(1)의 extra space complexity(결과값 제외한 공간 복잡도)를 가지도록 알고리즘을 만들어라.
- 나눗셈 연산을 하지 말아라.
- N은 [2, 10,000]
- 요소의 값은 [-30, 30]

# 문제접근

1. 결과의 i번 째에는 prefix(주어진 array의 1부터 i-1 번째까지의 누적곱) \* suffix(i+1부터 끝까지의 누적곱)이 들어가야한다. 왼쪽(색인이 낮은 쪽)에서 오른쪽(색인이 높은쪽)으로 prefix 누적곱을 구해준다. 그리고 오른쪽에서 왼쪽으로의 suffix 누적곱을 구하면서 미리 구해높은 prefix 누적곱과 곱해 결과값을 만들 수 있다.

# 코드

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var productExceptSelf = function (nums) {
	const result = Array(nums.length).fill(1);
	let cumMul = 1;
	for (let i = 0; i < result.length; i++) {
		cumMul *= nums[i];
		result[i] = cumMul;
	}

	cumMul = 1;
	for (let i = 0; i < result.length; i++) {
		const prefix =
			result.length - 2 - i > -1 ? result[result.length - 2 - i] : 1;
		result[result.length - 1 - i] = prefix * cumMul;
		cumMul *= nums[result.length - 1 - i];
	}

	return result;
};
```
