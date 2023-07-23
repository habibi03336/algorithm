---
title: LeetCode, 1. Two Sum

date: 2022-10-7

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

길이가 N인 integer로 이루어진 array와 target integer가 주어진다. array의 두 요소를 합하여 target number를 만들려고 한다. 반드시 하나의 답이 있다고 할 때 target number를 만드는 두 요소의 인덱스를 array로 반환하라.

- N은 10^4
- N의 요소 범위: -10^9 ~ 10^9
- target 범위: -10^9 ~ 10^9

# 문제접근

1. 시간 복잡도 상 O(N)이나 O(N\*logN)으로 접근해야 했다. 또한 요소의 범위가 상당히 넓기 때문에 짝이 될 수 있는 요소가 있는지 확인하기 위해 각 요소를 array의 index와 바인딩 하는 것도 어려운 상황이었다.

2. Hash table을 활용하여 짝이 되는 요소가 있는지 확인해보는 방식을 생각해보았다. 요소 카운팅 Hash table을 만드는데 O(N), 그리고 짝이 있는지 조회에 O(N)이라서 총 O(N)으로 접근 가능하다고 생각했다.

3. sorting이 된 경우 Caterpillar 방식으로 접근할 수 있다. sorting에 O(N\*logN), 병렬적으로 caterpillar 방식이 O(N)이라서 총 O(N\*logN)으로 접근 가능하다.

- 작은 부분(left)와 큰 부분(right)를 더해서 target 보다 크면 right를 하나 왼쪽으로 당기고, target보다 크면 left를 오른쪽으로 한 칸 움직이는 방식으로 left + right === target인 것을 O(N)으로 찾아갈 수 있다. 이에 대한 justification은 `2022-09-24-codility-minAbsSumOfTwo`에서 다루었다.

# 코드

## Hash table 방식

Runtime: 90ms / Memory: 45.1MB

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function (nums, target) {
	const counts = {};
	nums.forEach((elem, idx) => {
		if (counts[elem] === undefined) {
			counts[elem] = [idx];
		} else {
			counts[elem].push(idx);
		}
	});

	for (let i = 0; i < nums.length; i++) {
		const elemPair = target - nums[i];
		if (counts[elemPair] === undefined) continue;
		if (elemPair === nums[i] && counts[elemPair].length === 1) continue;
		if (elemPair === nums[i] && counts[elemPair].length >= 2) {
			return counts[elemPair].slice(0, 2);
		}
		return [i, counts[elemPair][0]];
	}
};
```

## Hash table 방식(최적화)

Runtime: 57ms / Memory: 42.8MB

보다 간단하게 다음과 같이 구현할 수 있다. 미리 numMap을 만들지 않고 맞는 짝이 없는 경우에만 기록을 해주어도 이후에 짝을 찾는데 문제가 없다. 또한 중복되는 요소가 있더라도 중복되는 요소로 target을 만들지 못하는 경우에만 numMap을 이후 인덱스로 업데이트 하기 때문에 문제가 없다.

```javascript
var twoSum = function (nums, target) {
	const numMap = {};
	for (let i = 0; i < nums.length; i++) {
		if (target - nums[i] in numMap) {
			return [numMap[target - nums[i]], i];
		}
		numMap[nums[i]] = i;
	}
};
```

## Caterpillar 방식

Runtime: 89ms / Memory: 45MB

```javascript
var twoSum = function (nums, target) {
	const numsWithIdx = nums.map((elem, idx) => [elem, idx]);
	numsWithIdx.sort((a, b) => a[0] - b[0]);
	let i = 0;
	let j = numsWithIdx.length - 1;
	while (i < j) {
		const left = numsWithIdx[i];
		const right = numsWithIdx[j];
		const sumOfTwo = left[0] + right[0];

		if (sumOfTwo > target) {
			j--;
		} else if (sumOfTwo < target) {
			i++;
		} else if (sumOfTwo === target) {
			return [left[1], right[1]];
		}
	}
};
```
