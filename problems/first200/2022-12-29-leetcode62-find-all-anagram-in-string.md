---
title: LeetCode, Find All Anagrams in a String

date: 2022-12-29
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/find-all-anagrams-in-a-string/description/)

문자열 s와 p가 주어진다. p와 anagram인 모든 s의 부분 문자열의 시작 인덱스를 반환하라.
(어떤 문자열 a의 문자의 순서를 바꾸어서 b를 만들 수 있으면 a와 b는 anagram 관계에 있다고 한다.)

- s.length와 p.length의 범위: [1, 30000]
- s와 p는 알파벳 소문자로만 이루어져 있다.

## 문제접근

1. 모든 경우의 수에 문자를 일일이 비교해가면서 anagram을 찾아줄 수 있다. 이렇게 하면 O(N^2)이라서 시간 복잡도가 높다. 따라서 몇 가지 트릭을 적용하는 것이 좋다. 하지만 아래와 같은 트릭을 적용했음에도 알고리즘의 한계 때문에 성능이 비교했을 때 최상으로 나오지 않는다.

1. 만약 s를 조회하는데 p에 아예 없는 문자를 만나게 될 경우, 그 사이의 경우를 뛰어넘어 해당 문자(없는 문자) 다음을 시작점으로 조회를 한다. (p에 없는 문자가 s 부분문자에 포함되어있으면 어차피 anagram이 될 수 없다.)
2. anagram을 찾은 경우, anagram의 맨 앞 문자와 s의 anagram 바로 뒤 문자를 비교하여 같으면 즉각적으로 해당 경우를 anagram으로 추가한다. (연속적으로 anagram이 있을 때 불필요하게 처음부터 다시 비교하지 않는다.)

2. 위 알고리즘의 문제는 각 경우에 대해서 매번 문자를 count한다는 것이다. 하지만 s의 부분 문자열은 앞 문자를 하나 빼고, 뒤에 문자를 하나 더하는 식으로 경우의 수를 늘려간다. 즉 앞선 부분 문자열과 이어지는 부분 문자열의 많은 문자가 겹친다는 것이다. 따라서 앞선 문자열의 count 결과를 사용한다면 O(N) 시간복잡도를 가지는 알고리즘을 만들 수 있다.

1. s의 부분 문자열 count 결과에서 빠지는 문자 1개에 대해서 개수를 줄이고, 새로 들어오는 문자에 대해서만 개수를 1개 늘리면 된다.
2. 각 문자에 대해서 같은 개수를 가지는지 확인해주면 된다. 알파벳 소문자만 있으므로 26번을 비교하면 anagram인지 확인해 줄 수 있다.

## 코드

### O(N^2) 알고리즘 with tricks : 비효율

```javascript
/**
 * @param {string} s
 * @param {string} p
 * @return {number[]}
 */
var findAnagrams = function (s, p) {
	const defaultCount = {};
	for (let i = 0; i < p.length; i++) {
		if (defaultCount[p[i]] === undefined) {
			defaultCount[p[i]] = 1;
		} else {
			defaultCount[p[i]] += 1;
		}
	}
	const result = [];
	for (let i = 0; i < s.length - p.length + 1; i++) {
		let count = { ...defaultCount };
		for (let j = 0; j < p.length; j++) {
			if (count[s[i + j]] === undefined) {
				i = i + j;
				break;
			}
			if (count[s[i + j]] === 0) {
				break;
			}
			if (j === p.length - 1) {
				result.push(i);
				let d = 1;
				while (s[i + d - 1] === s[i + j + d]) {
					result.push(i + d);
					d++;
				}
				i += d - 1;
			}
			count[s[i + j]] -= 1;
		}
	}
	return result;
};
```

### O(N) 알고리즘 : 효율

```javascript
const isAnagram = (arr1, arr2) => {
	for (let i = 0; i < arr1.length; i++) if (arr1[i] !== arr2[i]) return false;
	return true;
};

var findAnagrams = function (s, p) {
	let sArr = new Array(26).fill(0);
	let pArr = new Array(26).fill(0);
	let res = [];

	for (let i = 0; i < p.length; i++) {
		let index = p.charCodeAt(i) % 26;
		pArr[index]++;
	}

	for (let i = 0; i < s.length; i++) {
		let index = s.charCodeAt(i) % 26;
		sArr[index]++;

		if (i > p.length - 1) {
			let headIndex = s.charCodeAt(i - p.length) % 26;
			sArr[headIndex]--;
		}
		if (i >= p.length - 1) {
			if (isAnagram(sArr, pArr)) res.push(i - (p.length - 1));
		}
	}

	return res;
};
```
