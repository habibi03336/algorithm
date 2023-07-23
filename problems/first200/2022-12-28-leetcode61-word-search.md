---
title: LeetCode, Word Search

date: 2022-12-28
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/word-search/description/)

m x n 인 board와 문자열 word가 주어진다. board에 있는 인접해 있는 문자를 이어서 문자열 word를 만들 수 있으면 true 없으면 false를 반환하라.

- board에서 상,하,좌,우에 붙어있는 문자가 인접한 문자이다.
- board에서 같은 문자를 중복해서 사용할 수는 없다.
- m, n의 범위는 [1, 6]
- 문자열 word의 길이의 범위는 [1, 15]

## 문제접근

1. 재귀함수를 통해, 모든 경우의 수를 검색해줄 수 있다.

## 구현방식

1. 여러 경우의 수 중에서 한 가지의 경우만 가능하면 된다. 따라서 논리 연산자 || 를 이용해 한 가지의 경우에만 가능해도 true를 반환하도록 한다.

2. 이미 사용한 board의 문자는 다시 고려되면 안 되기 때문에 임시적으로 null을 넣어서 막아놓고, 해당 경우의 수에 대한 검색이 끝나면 다시 값을 복구해준다.

## 코드

```javascript
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */
const dfs = (board, i, j, word, c) => {
	if (word.length === c) return true;
	if (
		board.length <= i ||
		board[0].length <= j ||
		i < 0 ||
		j < 0 ||
		board[i][j] !== word[c]
	)
		return false;
	const tmp = board[i][j];
	board[i][j] = null;
	const result =
		dfs(board, i + 1, j, word, c + 1) ||
		dfs(board, i, j + 1, word, c + 1) ||
		dfs(board, i - 1, j, word, c + 1) ||
		dfs(board, i, j - 1, word, c + 1);
	board[i][j] = tmp;
	return result;
};

var exist = function (board, word) {
	for (let i = 0; i < board.length; i++) {
		for (let j = 0; j < board[0].length; j++) {
			if (dfs(board, i, j, word, 0)) return true;
		}
	}
	return false;
};
```
