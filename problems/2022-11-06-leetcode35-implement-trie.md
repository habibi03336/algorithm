---
title: LeetCode, Implement Trie (prefix tree)

date: 2022-11-6
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/implement-trie-prefix-tree/)

단어를 insert, search 그리고 해당 prefix를 가진 단어가 있는지 확인하는 startWith 메소드가 있는 trie 자료구조를 구현하라.

- 단어는 영어 소문자만으로 이루어져있다.
- 단어의 길이는 [1, 2000]
- 메소드 호출은 최대 3,000번 이루어진다.

# 문제접근

1. [Trie 자료구조](https://www.geeksforgeeks.org/trie-insert-and-search/)를 구현하는 문제였다. Trie는 단어 자동완성이나, 스펠링 체크 등의 기능에 많이 쓰이는 자료구조이다. 단어 길이가 N이라고 할 때, insert, search 모두 O(N)이다.

2. 영어 소문자만 있다는 조건을 활용하여 array index와 ascii코드를 활용하여 자식 노드의 포인터를 저장해주었다.

# 코드

```javascript
var Trie = function () {
	this.children = Array(26).fill();
	this.isEndOfWord = false;
};

/**
 * @param {string} word
 * @return {void}
 */
Trie.prototype.insert = function (word) {
	let node = this;
	for (let i = 0; i < word.length; i++) {
		const charIdx = word.charCodeAt(i) - 97;
		if (node.children[charIdx] === undefined) {
			node.children[charIdx] = new Trie();
		}
		node = node.children[charIdx];
		if (i === word.length - 1) {
			node.isEndOfWord = true;
		}
	}
};

/**
 * @param {string} word
 * @return {boolean}
 */
Trie.prototype.search = function (word) {
	let node = this;
	for (let i = 0; i < word.length; i++) {
		const charIdx = word.charCodeAt(i) - 97;
		if (node.children[charIdx] === undefined) {
			return false;
		}
		node = node.children[charIdx];
		if (i === word.length - 1) {
			return node.isEndOfWord;
		}
	}
};

/**
 * @param {string} prefix
 * @return {boolean}
 */
Trie.prototype.startsWith = function (prefix) {
	let node = this;
	for (let i = 0; i < prefix.length; i++) {
		const charIdx = prefix.charCodeAt(i) - 97;
		if (node.children[charIdx] === undefined) {
			return false;
		}
		node = node.children[charIdx];
	}

	return true;
};

/**
 * Your Trie object will be instantiated and called as such:
 * var obj = new Trie()
 * obj.insert(word)
 * var param_2 = obj.search(word)
 * var param_3 = obj.startsWith(prefix)
 */
```
