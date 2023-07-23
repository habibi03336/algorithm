---
title: LeetCode, Binary Search

date: 2022-10-13

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

각 요소가 정수이고, 증가하는 순으로 정렬된 array가 주어진다. 찾아내야하는 target이 주어졌을 때 array에 존재하면 해당 index를 반환하고 없다면 -1을 반환하라.

- array의 length는 10,000
- 각 요소는 unique

# 문제접근

1. 정렬이 되어있기 때문에 이진탐색으로 O(logN)으로 찾아낼 수 있다.

# 코드

- 이진탐색은 무한루프 도는 것을 조심해야하는데 아래 코드의 기본형식에 따라 작성하면 무한루프가 돌지 않는다.
- 이진탐색은 1. 최저, 최고값이 정해져 있고 2. 값의 도메인이 유한개이며 3.선형적인 크기 관계로 답에 접근할 수 있을 때 응용할 수 있다. 
- 관련 문제, 이진탐색 응용: `2022-09-16-codility-nailingPlanks`

```javascript
var search = function (nums, target) {
	let lo = 0;
	let hi = nums.length - 1;
	while (lo <= hi) {
		const mid = Math.floor((lo + hi) / 2);
		if (nums[mid] === target) return mid;
		if (nums[mid] > target) {
			hi = mid - 1;
		} else {
			lo = mid + 1;
		}
	}

	return -1;
};
```
