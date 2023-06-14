---
title: LeetCode, 42. Trapping Rain Water

date: 2023-1-16

categories:
  - 문제풀이

tags:
  - Exercise
  - 🧑🏻‍💻
---

# [문제요약](https://leetcode.com/problems/trapping-rain-water/description/)

양의 정수가 담겨있는 길이가 n인 배열이 주어진다. 각 요소는 너비가 1인 기둥의 높이를 나타낸다. 얼마나 많은 물이 기둥 사이에 모일 수 있는지를 계산하라.

- n의 범위: [1, 20,000]

# 문제접근

1. 선형자료구조이고, 브루트포스로 접근하기가 어렵다. 이 때 투 포인터 풀이를 고려해 볼 수 있다.

2. 두 기둥(투 포인터)이 가지는 최소의 높이(채울 수 있는 물의 높이 최대)을 구하고, 한 칸씩 들어가는 물의 양을 구해주는 방식으로 접근할 수 있다.

# 구현

1. minH는 두 기둥 중에 작은 기둥의 높이를 의미한다. 두 기둥 사이에는 작은 기둥 높이 만큼 물을 채울 수 있다. minH는 왼쪽, 혹은 오른쪽 기둥의 높이에 변화가 있어서 두 기둥 사이 채울 수 있는 물의 높이가 증가하는 경우에 업데이트된다. minH는 커지는 방향으로만 업데이트 된다. 왜냐하면 이미 양쪽에 더 낮은 기둥에 대해서 채울 수 있는 물의 양은 줄어들지 않기 때문이다.

2. minH가 정해지면, 해당 index에 들어가는 물의 양을 계산한다. 그리고 반영이되면 해당 index는 더 고려할 필요가 없기 때문에 다음 index로 업데이트 한다. (p1,p2를 동시에 하면 n이 홀수 일 때, 중간 요소를 빼먹게 되므로 하나씩만 업데이트한다.)

# 코드

```javascript
/**
 * @param {number[]} height
 * @return {number}
 */
var trap = function (height) {
	let p1 = 0;
	let p2 = height.length - 1;
	let minH = 0;
	let water = 0;
	while (p1 < p2) {
		minH = Math.max(minH, Math.min(height[p1], height[p2]));
		if (height[p1] <= minH) {
			water += minH - height[p1];
			p1 += 1;
			continue;
		}
		if (height[p2] <= minH) {
			water += minH - height[p2];
			p2 -= 1;
		}
	}
	return water;
};
```
