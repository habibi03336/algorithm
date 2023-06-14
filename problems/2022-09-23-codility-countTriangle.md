---
title: Codility, Count Triangles

date: 2022-9-23

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

길이가 N인 양인 정수로 이루어진 배열 A가 주어진다. 각 요소가 삼각형의 한 변을 의미할 때 각 요소를 조합해서 만들 수 있는 삼각형의 개수를 구하여라.

- 삼각형이 되기 위해서는 (가장 큰 수) > (다른 두 수의 합) 이어야 한다.
- N은 0 ~ 1,000이다.

# 문제접근

1. N이 1,000이기 때문에 O(N^2) 정도로 접근을 해도 된다.

2. 3가지 요소에서 가장 큰 수가 무엇인지 중요하기 때문에 미리 정렬하는 것이 좋을 것이라 생각했다.

3. 가장 큰 수가 주어진 상태에서, 작은 부분에서 하나(lo) 큰 부분에서 하나(hi) 뽑아서 좁혀가는 애벌레 방식을 적용할 수 있다. 만약 lo요소 + hi요소가 biggest보다 크다는 조건을 만족하면 hi에서 뽑은 요소 + (lo와 lo~hi 사이의 모든 요소)는 조건을 만족한다. 반대로 lo요소 + hi요소가 조건을 만족하지 못한다면 lo에서 뽑은 요소 + (hi요소보다 작은 모든 요소)는 조건을 만족하지 못한다.

- 가장 큰 수로 N만큼 가정하고, 각 가정에 대해서 O(N)의 애벌레 방식을 적용하기 때문에 총 O(N^2)이다.

# [코드](https://app.codility.com/demo/results/trainingMPDYNP-4DJ/)

```javascript
function solution(A) {
	A.sort((a, b) => a - b);
	let count = 0;
	while (A.length >= 3) {
		const biggest = A.pop();
		let lo = 0;
		let hi = A.length - 1;
		while (lo < hi) {
			if (A[lo] + A[hi] > biggest) {
				count += hi - lo;
				hi--;
			} else {
				lo++;
			}
		}
	}

	return count;
}
```
