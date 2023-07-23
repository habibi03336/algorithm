---
title: Codility, Number Solitaire

date: 2022-10-1

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

정수로 이루어진 길이가 N인 배열 A가 주어진다. 0에서 시작해 1~6까지 적힌 주사위를 던저서 나온 수 만큼 앞으로 이동한다. 마지막 요소에 도착하면 게임이 끝난다. 만약 유효한 인덱스를 넘어서면 마지막 인덱스에 도달했다고 여긴다. 거쳐간 인덱스의 요소의 총합을 구한다고 할 때 가능한 가장 큰 값은 무엇인가?

# 문제접근

1. 마지막 요소에 도달하기 전 거쳐올 수 있는 요소는 앞 선 6개이다. 최대 값을 구하기 위해서는 앞 선 6개 요소에서 가장 큰 총합을 만드는 요소를 거쳐와야 한다.

- 앞 선 6개 인덱스까지 총합을 구하는 더 작은 문제로 바뀐다. 그리고 이것은 앞 선 6개 인덱스에 대해서도 똑같이 적용된다.

2. O(N) \* O(6)이므로 O(N)이다.

# [코드](https://app.codility.com/demo/results/trainingST7JA3-ZRF/)

```javascript
function solution(A) {
	const localResults = Array(A.length).fill(A[0]);
	for (let i = 1; i < A.length; i++) {
		let localOptimal = Number.MIN_SAFE_INTEGER;
		for (let j = 0; j < 6 && j < i; j++) {
			const localCase = A[i] + localResults[i - (j + 1)];
			if (localOptimal < localCase) {
				localOptimal = localCase;
			}
		}
		localResults[i] = localOptimal;
	}

	return localResults[localResults.length - 1];
}
```
