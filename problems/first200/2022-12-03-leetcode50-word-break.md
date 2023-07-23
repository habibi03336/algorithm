---
title: LeetCode, Word Break

date: 2022-12-3
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/word-break/)

하나의 문자열 s와 길이가 N인 문자열 배열인 wordDict가 주어진다. 문자열 s가 wordDict의 문자들로 분리 될 수 있으면 true 아니면 false를 반환하라.
(단, s를 분리한 결과에 wordDict에 있는 한 단어가 여러번 쓰여도 된다.)

- s.length: [1, 300]
- wordDict.length: [1, 300]
- wordDict\[i\].length: [1, 20]
- s와 wordDict 요소의 문자열은 모두 영어 소문자로만 이루어져 있다.
- wordDict의 문자열 요소는 unique하다.

```javascript
const s = "penpineappleapplepen";
const wordDicst = ["pen", "pineapple", "apple"];
const result = true;
```

# 문제접근

1. wordDict의 단어들을 트리 구조의 일종인 trie 자료구조를 이용하여서 표현할 수 있다.

2. 재귀함수를 통해서 문자열 s의 문자를 순회하면서, 여러가지 경우의 수에 대해서 모두 따져줄 수 있다. wordDict 단어가 끝날 수 있을 때, 끝내는 경우 그리고 끝내지 않고 계속 조회해보는 경우 2가지로 분기한다.

3. 최악의 경우 O(2^s.length)이기 때문에 시간복잡도가 너무 커진다. 따라서 재귀함수를 최대한 덜 호출하도록 조건을 걸어주는 것이 좋다. 이 문제의 경우 단어가 끝나서 다음 지점에서 부터 다른 단어를 쓸 수 있을 때 새로운 분기가 나오게 된다. 이 때 이미 앞서서 고려된 새로운 시작 지점이라면 또 확인할 필요가 없다. 따라서 이미 고려된 시작 지점인지 확인하여, 그렇지 않은 경우에만 새로운 경우의 수로 분기하여 확인하도록 했다.

# 구현 방식

1. trie를 배열로만 구현했다. 배열의 요소가 null이면 해당 문자를 가지고 있지 않은 것으로, null이면 가지고 있는 것으로 두었다. 또한 null이 아니면 그 포인터가 가르키는 배열이 새로운 node를 가르키는 것으로 구현했다. 다만 이런 식으로 하니까 다소 복잡하게 느껴졌다. 각 값이 무엇을 나타내는 것인지 직관적으로 파악이 안 된다.

- 트리 구조에서는 부모가 자식을 조회할 수 있기 때문에, 부모의 입장에서 자식 노드가 있는지 확인하고, 자식노드의 조건에 따라 제어를 해준다.

```javascript
//만약 a, b, c만 있다고 하면
const s = "abc";
const wordDict = ["a", "bc"];

// trie를 구성하면 다음과 같은 구조를 가지게 된다.
// root   :    [[[Array(3), true], [Array(3), false], null], false];
// root[0]:    [[null, null, null], true]
// root[1]:    [[null, null, [Array(3), true]], false]
// root[1][0]: [[null, null, null], true]
```

2. 객체를 활용하여서 값을 보다 직관적으로 정리할 수 있을 것이라고 생각을 했다. trie의 각 노드를 나타내는 객체를 하나 만들어서 구현했다. 또한 문자열이 있는지 확인하기 위해 배열 대신 hash table을 활용했다. 자바스크립트는 hash table을 사용하기 굉장히 편리한 언어이다.

# 코드

## 배열을 활용한 구현

111 ms 44.6 MB

```javascript
/**
 * @param {string} s
 * @param {string[]} wordDict
 * @return {boolean}
 */
var wordBreak = function (s, wordDict) {
	//97 - 122
	const letters = Array(26).fill(null);
	const trieRoot = [letters.slice(), false];
	const makeTrie = (word, trieNode, i) => {
		if (word.length <= i) return;
		const code = word.charCodeAt(i) - 97;
		if (trieNode[0][code] === null)
			trieNode[0][code] = [letters.slice(), false];
		if (word.length - 1 === i) trieNode[0][code][1] = true;
		makeTrie(word, trieNode[0][code], i + 1);
	};
	for (let i = 0; i < wordDict.length; i++) {
		makeTrie(wordDict[i], trieRoot, 0);
	}
	let isAvailable = false;
	const checkedStart = Array(s.length).fill(false);
	const recursiveChecker = (s, trieNode, i, rootNode) => {
		if (s.length <= i) return;
		const code = s.charCodeAt(i) - 97;
		if (trieNode[0][code] === null) return;
		if (trieNode[0][code][1] === false) {
			recursiveChecker(s, trieNode[0][code], i + 1, rootNode);
		}
		if (trieNode[0][code][1] === true) {
			recursiveChecker(s, trieNode[0][code], i + 1, rootNode);
			if (checkedStart[i + 1] === false) {
				checkedStart[i + 1] = true;
				recursiveChecker(s, rootNode, i + 1, rootNode);
			}
		}
		if (s.length - 1 === i && trieNode[0][code][1]) {
			isAvailable = true;
		}
	};
	recursiveChecker(s, trieRoot, 0, trieRoot);
	return isAvailable;
};
```

## 객체와 hashmap을 활용한 구현

132 ms 46.4 MB

```javascript
var wordBreak = function (s, wordDict) {
	function TrieNode() {
		this.isEndPoint = false;
		this.chars = {};
	}
	const trieRoot = new TrieNode();
	for (let i = 0; i < wordDict.length; i++) {
		let trieNode = trieRoot;
		for (let j = 0; j < wordDict[i].length; j++) {
			if (trieNode.chars[wordDict[i][j]] === undefined) {
				trieNode.chars[wordDict[i][j]] = new TrieNode();
			}
			if (j === wordDict[i].length - 1) {
				trieNode.chars[wordDict[i][j]].isEndPoint = true;
			}
			trieNode = trieNode.chars[wordDict[i][j]];
		}
	}
	let isAvailable = false;
	const checkedStart = Array(s.length).fill(false);
	const recursiveChecker = (s, trieNode, i, rootNode) => {
		if (s.length <= i) return;
		if (trieNode.chars[s[i]] === undefined) return;
		recursiveChecker(s, trieNode.chars[s[i]], i + 1, rootNode);
		if (trieNode.chars[s[i]].isEndPoint) {
			if (checkedStart[i + 1] === false) {
				checkedStart[i + 1] = true;
				recursiveChecker(s, rootNode, i + 1, rootNode);
			}
		}
		if (s.length - 1 === i && trieNode.chars[s[i]].isEndPoint) {
			isAvailable = true;
		}
	};
	recursiveChecker(s, trieRoot, 0, trieRoot);
	return isAvailable;
};
```

# 추가사항

이런 방식으로 DP 풀이도 가능한 것을 발견했다.

```javascript
var wordBreak = function (s, wordDict) {
	const dp = [];
	dp[s.length] = true;
	for (let i = s.length - 1; i >= 0; --i) {
		for (const word of wordDict) {
			if (
				i + word.length <= s.length &&
				s.substring(i, i + word.length) === word &&
				dp[i + word.length]
			) {
				dp[i] = true;
				break;
			}
		}
	}
	return dp[0] || false;
};
```
