---
title: LeetCode, Ransom Note

date: 2022-10-22
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/ransom-note/)

두 문자열 ransomNote와 magazine이 주어진다. magazine의 문자를 한 번씩 써서 ransomNote 문자열을 만들 수 있으면 true 없으면 false를 반환하라.

- ransomNote, magazine 문자열의 길이는 100,000
- 두 문자열은 모두 영어 소문자만으로 이루어져있다.

# 문제접근

1. magazine 문자열의 문자를 count한 후 count한 결과를 이용해서 ransomNote 문자열을 만들 수 있는지 확인해 주면 된다. (O(Max(ransomNote.length, magazine.length))) array index와 ascii 코드를 바인딩하여 문자를 count했다. 영어 알파벳 소문자만으로 이루어져있다는 조건이 있어서 이러한 접근이 쉬월했다.

2. ascii코드를 사용하지 않고 hash table을 활용하여 count 할 수도 있다. 다만 hash table의 경우 기본적인 오버헤드가 좀 있는 편이다. 얼마나 차이가 나는지 궁금해서 hash table로도 코드를 작성해 봤다.

# 코드

## array index + ascii code 접근

64 ms, 42.8 MB

```javascript
var canConstruct = function (ransomNote, magazine) {
	const wordsCount = Array(26).fill(0);
	for (let i = 0; i < magazine.length; i++) {
		wordsCount[magazine.charCodeAt(i) - 97] += 1;
	}

	for (let i = 0; i < ransomNote.length; i++) {
		const ascii = ransomNote.charCodeAt(i);
		if (wordsCount[ascii - 97] === 0) return false;
		wordsCount[ascii - 97] -= 1;
	}

	return true;
};
```

## hash table 접근

163 ms 44.8 MB

```javascript
var canConstruct = function (ransomNote, magazine) {
	const wordsCount = {};
	for (let i = 0; i < magazine.length; i++) {
		if (wordsCount[magazine[i]] === undefined) {
			wordsCount[magazine[i]] = 1;
		} else {
			wordsCount[magazine[i]] += 1;
		}
	}

	for (let i = 0; i < ransomNote.length; i++) {
		if (!wordsCount[ransomNote[i]]) return false;
		wordsCount[ransomNote[i]] -= 1;
	}

	return true;
};
```

속도 차이가 거의 3배 정도 난다. 확실히 오버헤드가 있음을 확인 할 수 있다. 그런데 자바스크립트의 경우 array가 유사 배열이라서 결국에는 hash table이다. 그럼에도 array가 looping에 더 빠르다. 내부 동작이 보다 최적화 되어 있는 것으로 보인다. [관련 글](https://medium.com/@mahesh_joshi/javascript-performance-array-vs-object-794f1e30e920)
