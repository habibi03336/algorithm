---
title: LeetCode, 17. Letter Combinations of a Phone Number

date: 2022-12-17
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

2부터 9까지의 digit이 들어있는 길이가 n인 문자열이 주어진다. 각 숫자가 여러 개의 소문자 알파벳과 매핑되어 있다. digit을 매핑된 알파벳으로 바꿀 때 가능한 모든 경우의 수를 반환하라.

- n의 범위: \[0, 4\]

## 문제접근

1. 유향 그래프 탐색 문제로 볼 수 있다. 각 숫자의 자리 수에 대해서 알파벳 노드가 있고, 다음 자리 수에 대응하는 알파벳 노드들 모두로 향하는 경로가 있는 그래프이다.

<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/habibi03336/algorithm/master/assets/img/leetcode-17.jpeg" alt="leetcode-17"/>
</div>

## 구현

1. dfs와 backtracking을 활용하여서 구현해줄 수 있다.

2. bfs의 개념으로 for문을 통해 level 단위로 접근하며 풀어줄 수 있다.

## 코드

### DFS, recursion을 활용한 구현

```javascript
/**
 * @param {string} digits
 * @return {string[]}
 */

const numCharMap = new Map([
	["2", ["a", "b", "c"]],
	["3", ["d", "e", "f"]],
	["4", ["g", "h", "i"]],
	["5", ["j", "k", "l"]],
	["6", ["m", "n", "o"]],
	["7", ["p", "q", "r", "s"]],
	["8", ["t", "u", "v"]],
	["9", ["w", "x", "y", "z"]],
]);

var letterCombinations = function (digits) {
	if (digits.length === 0) return [];
	const res = [];
	const dfs = (index, words) => {
		if (words.length === digits.length) {
			res.push(words);
			return;
		}
		const chars = numCharMap.get(digits[index]);
		for (let char of chars) {
			dfs(index + 1, words + char);
		}
	};
	dfs(0, "");
	return res;
};
```

### BFS, for문을 활용한 구현

```javascript
/**
 * @param {string} digits
 * @return {string[]}
 */
const numCharMap = new Map([
	["2", ["a", "b", "c"]],
	["3", ["d", "e", "f"]],
	["4", ["g", "h", "i"]],
	["5", ["j", "k", "l"]],
	["6", ["m", "n", "o"]],
	["7", ["p", "q", "r", "s"]],
	["8", ["t", "u", "v"]],
	["9", ["w", "x", "y", "z"]],
]);
var letterCombinations = function (digits) {
	let combinations = [];
	for (let i = 0; i < digits.length; i++) {
		const newCombinations = [];
		const possibleChars = numCharMap.get(digits[i]);
		for (let j = 0; j < possibleChars.length; j++) {
			const char = possibleChars[j];
			if (combinations.length === 0) {
				newCombinations.push(char);
			} else {
				for (let k = 0; k < combinations.length; k++) {
					newCombinations.push(combinations[k] + char);
				}
			}
		}
		combinations = newCombinations;
	}
	return combinations;
};
```
