---
title: Codility, Min Perimeter Rectangle

date: 2022-9-3

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

사각형의 넓이 N에 대하여, 사각형이 가질 수 있는 둘레의 최소값은? (단, 사각형의 변의 길이는 정수이다.)

# 문제접근

사각형의 각 변 길이의 차이가 적을 수록 둘레가 작아진다. 따라서 사각형 넓이의 약수 중 가장 마지막 쌍을 두 변의 길이로 하면 된다.

# 코드

```javascript
function solution(N) {
	const sqrtN = Math.floor(Math.sqrt(N));
	let side;
	for (let i = sqrtN; i > 0; i--) {
		if (N % i === 0) {
			side = i;
			break;
		}
	}

	const perimeter = 2 * (side + Math.floor(N / side));

	return perimeter;
}
```
