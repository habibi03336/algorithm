---
title: LeetCode, Longest Palindromic Substring

date: 2022-12-13
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/longest-palindromic-substring/description/)

길이가 N인 문자열 s가 주어진다. s의 substring 중 가장 긴 palindrome을 반환하라. (철자를 앞에서부터 읽던 뒤에서부터 읽던 똑같은 것을 palindrome이라고 한다.)

- N의 범위: [1, 1000]
- 가장 길고, 길이가 같은 palindrome이 여러 개 있으면 아무거나 반환하면 된다.

# 방식1. 가장 긴 문자열에서 시작해서, 앞 뒤 문자를 제거해가며 모든 경우의 수를 확인해주는 방법 (비효율적)

## 문제접근

1. "모든 substring 경우의 수(O(N^2))"에 대해서 "palindrome인지 확인(O(N))"해 준다. 따라서 O(N^3)이 된다.

2. palindrome 길이 별로 BFS 개념으로 접근하기 때문에 가장 긴 parlindrome을 찾으면 바로 반환할 수 있다.

## 구현 방식

1. BFS를 하면서 길이 단위로 끊어서 확인한다. while문 안에서 for문과 dequeLen를 활용하여 길이 별로 제어해준다.

2. 중복을 허용하여 분기를 하면 O(2^N \* N)이 되기 때문에 Set 객체를 활용하여 중복을 제거했다.

## 코드

9704 ms, 86 MB

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function (s) {
	const isPalindrome = (start, end) => {
		while (start <= end) {
			if (s[start] !== s[end]) {
				return false;
			}
			start++;
			end--;
		}
		return true;
	};
	const checker = new Set();
	const deque = [[0, s.length - 1]];
	while (deque.length !== 0) {
		const dequeLen = deque.length;
		for (let i = 0; i < dequeLen; i++) {
			const [start, end] = deque.shift();
			if (start === end) {
				return s[start];
			}
			if (isPalindrome(start, end)) {
				return s.substring(start, end + 1);
			} else {
				if (!checker.has(`${start + 1}:${end}`)) {
					checker.add(`${start + 1}:${end}`);
					deque.push([start + 1, end]);
				}
				if (!checker.has(`${start}:${end - 1}`)) {
					checker.add(`${start}:${end - 1}`);
					deque.push([start, end - 1]);
				}
			}
		}
	}
};
```

# 방식2. 중앙이 되는 하나의 pivot 문자에서 시작해서, 앞 뒤 문자를 더해주면서 모든 경우의 수를 확인해주는 방법 (효율적)

## 문제접근

1. "각 문자가 중앙(pivot)인 경우의 수(O(N))"에 대해서 각 경우에 "가장 긴 palindrome을 만들고(O(N))" 최대 길이를 가지면 "최종 결과를 저장하는 변수에 재할당(O(1))"한다. 따라서 총 O(N^2)이다.

2. 발견: 연속적으로 같은 문자가 있다면 반드시 하나의 substring안에 모두 포함되는 것이 가장 긴 palindrome을 만든다.

3. 연속적으로 pivot과 같은 문자가 있는 색인까지는 확인을 건너뛴다. 이렇게 설정하면 `1) 앞쪽으로 pivot과 같은 연속적인 문자는 없다는 의미`, `2) 뒤쪽으로 pivot과 연속하여 같은 문자가 있으면 일단 포함하는 것이 최대 길이를 만드는 기본 세팅이 된다는 의미` 이 두 가지를 알 수 있다. (caterpillar 개념)

## 코드

78 ms 42.9 MB
방식2가 방식1보다 약 125배 빨랐다. 메모리 성능도 2배 정도 효율적이었다.

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function (s) {
	let longest = "";
	for (let i = 0; i < s.length; i++) {
		let start = i;
		let end = i;
		while (end + 1 < s.length && s[end] === s[end + 1]) {
			end++;
			i++;
		}
		while (start >= 0 && end < s.length) {
			if (s[start] !== s[end]) break;
			start--;
			end++;
		}
		if (longest.length < end - start - 1) {
			longest = s.substring(start + 1, end);
		}
	}
	return longest;
};
```
