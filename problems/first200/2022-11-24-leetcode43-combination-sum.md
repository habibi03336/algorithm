---
title: LeetCode, Combination Sum

date: 2022-11-24
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/combination-sum/)

숫자가 담긴 array와 목표가 되는 target 숫자가 주어진다. array에 담긴 숫자로 target을 만든다고 할 때 가능한 모든 경우의 수를 2차원 배열로 반환하라.

- array에 담긴 수를 여러 번 쓸 수 있다.

# 문제접근

1. 이전의 케이스에서 가지를 뻗어가며 경우의 수를 확장하고, 조건을 만족하는지 확인해 줄 수 있다고 생각했다. 이런 생각에 다다르니 recursion을 활용해야겠다는 생각이 들었다.

2. 중복되는 경우를 제외하기 위해서 오름차순을 만족하는 경우만 recursion 함수를 호출 하도록 했다.

# 코드

## 다소 비효율적인 코드

재귀함수를 호출 할 때 매번 새로운 배열을 만들어 넣어주었다. 배열을 매번 초기화하기 때문에 시간, 공간 성능 모두 좋지 않았다.

165 ms 50.1 MB

```javascript
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum = function (candidates, target) {
	const results = [];
	const recursiveSearch = (subResult) => {
		const subTotal = subResult.reduce((pre, cur) => pre + cur, 0);
		if (subTotal === target) {
			results.push(subResult);
			return;
		}
		if (subTotal > target) {
			return;
		}

		candidates.forEach((elem) => {
			if (subResult.length === 0 || subResult[subResult.length - 1] <= elem) {
				recursiveSearch([...subResult, elem]);
			}
		});
	};
	recursiveSearch([]);
	return results;
};
```

## 개선된 코드

144 ms 44.9 MB

결과값을 저장할 때만 새로운 배열을 생성하는 코드로 바꾸었다. 탐색 중에는 기존 배열에 하나의 요소만 넣었다 뺐다 하는 식으로 구현했다. 재귀를 구현할 때 이런 지점을 좀 신경 써주면 조금 더 효율적인 코드를 쓸 수 있을 것이다.

```javascript
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum = function (candidates, target) {
	const results = [];
	const recursiveSearch = (subResult) => {
		const subTotal = subResult.reduce((pre, cur) => pre + cur, 0);
		if (subTotal === target) {
			results.push([...subResult]);
			return;
		}
		if (subTotal > target) {
			return;
		}

		candidates.forEach((elem) => {
			if (subResult.length === 0 || subResult[subResult.length - 1] <= elem) {
				subResult.push(elem);
				recursiveSearch(subResult);
				subResult.pop();
			}
		});
	};
	recursiveSearch([]);
	return results;
};
```
