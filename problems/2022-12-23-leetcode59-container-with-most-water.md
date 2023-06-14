---
title: LeetCode, Container With Most Water

date: 2022-12-17
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/container-with-most-water/)

기둥의 높이가 들어있는 길이가 N인 배열이 주어진다. 배열의 순서대로 1의 간격을 두고 기둥이 박혀있다. 두 기둥을 선택하여 물을 채울 때, 가장 많은 물의 양을 반환하라.
(그림을 보는 편이 이해가 빠름)

- N의 범위: \[2, 100,000\]

## 문제접근

1. 문제를 효율적으로 풀기 위해서 caterpillar 방식을 사용할 수 있다.

2. 기둥 두 개를 선택했다고 하면, (둘 중에 작은 높이) \* (기둥의 간격) 으로 물의 양이 정해진다.

3. 처음과 끝 두 개의 포인터가 있다고 할 때, 둘 중 작은 쪽의 포인터를 움직이는 것만이 물의 양이 늘어나는 변화이다.

### justification

1. 양 끝에서 시작하므로 포인터를 움직이면 간격은 계속 줄어든다.
2. 높이는 두 기둥 중 작은 쪽으로 정해진다.
3. 큰 쪽을 움직이면, 줄어든 간격 \* Min(작은 기둥의 높이, 새로운 기둥 높이)이기 때문에 기존의 물의 양보다 적어질 수 밖에 없다.

## 코드

```javascript
/**
 * @param {number[]} height
 * @return {number}
 */
var maxArea = function (heights) {
	let i = 0;
	let j = heights.length - 1;
	let currentMax = 0;
	while (i < j) {
		const height = Math.min(heights[i], heights[j]);
		const width = j - i;
		const area = height * width;

		currentMax = Math.max(currentMax, area);

		if (heights[i] <= heights[j]) {
			i++;
		} else {
			j--;
		}
	}
	return currentMax;
};
```
