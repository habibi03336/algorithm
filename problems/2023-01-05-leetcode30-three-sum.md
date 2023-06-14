---
title: LeetCode, Three Sum

date: 2023-1-5
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/3sum/description/)

숫자가 담긴 길이 n의 배열이 주어진다. 배열의 세 수를 합쳐 0이 되는 모든 경우의 세 수를 반환하라.

- 중복되는 경우는 포함하지 않는다.

## 문제접근

1. 한 숫자를 고정해 놓고, two sum을 풀 듯이 투 포인터로 풀어 줄 수 있다. 고정되는 숫자의 개수가 O(N)이고 각 경우에 대한 투 포인터가 O(N)이기 때문에 O(N^2)으로 풀린다.

2. 중복을 없애야한다. 따라서 만약 연속적으로 같은 수가 나오는 경우에 대해서 건너뛰어준다.

## 코드

```javascript
var threeSum = function (nums) {
	nums.sort((a, b) => a - b);
	const res = [];
	for (let i = 0; i < nums.length - 2; i++) {
		if (i > 0 && nums[i] === nums[i - 1]) continue;
		let j = i + 1;
		let k = nums.length - 1;
		while (j < k) {
			const sum = nums[i] + nums[j] + nums[k];
			if (sum < 0) {
				j++;
			}
			if (sum > 0) {
				k--;
			}
			if (sum === 0) {
				res.push([nums[i], nums[j], nums[k]]);
				while (nums[j] === nums[j + 1]) {
					j++;
				}
				while (nums[k] === nums[k - 1]) {
					k--;
				}
				j++;
				k--;
			}
		}
	}
	return res;
};
```
