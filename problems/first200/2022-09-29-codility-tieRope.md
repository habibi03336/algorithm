---
title: Codility, Tie Rope

date: 2022-9-29

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

길이가 N인 배열이 주어진다. 각 배열의 요소는 순서대로 나열되어있는 밧줄의 길이를 의미한다. 서로 인접한 밧줄 끼리 묶어서 더 긴 밧줄을 만들 수 있다. 묶인 밧줄을 또 다른 밧줄과도 묶을 수 있다. K가 주어질 때, K보다 긴 밧줄을 최대 몇 개 만들 수 있는가?

- N은 1 ~ 100,000
- 배열의 각 요소와 K는 1 ~ 1,000,000,000

# 문제접근

1. 그리디한 방식으로 접근했다. 순서대로 접근하면서 K보다 작은 경우 묶고, K보다 크거나 같은 경우 카운팅을 해주면 최대 값을 구할 수 있다.

- 0 보다 큰 요소만 있기 때문에 앞서 존재 하는 요소는 총합을 크게 하는데 반드시 도움이 된다. 앞 선 요소를 포함하는 것이 항상 최적이다. 따라서 계속 더해주면서 조건을 확인해주면 된다.

# 만약에

묶는 작업을 최소로 한다는 조건이 붙었으면 어땠을까? K보다 크거나 같은 값에 도달 하면 맨 앞에서 부터 하나씩 뺴보면서 여전히 조건을 만족하는지 확인하는 방식으로 접근 할 수 있다. 그렇게 해도 여전히 O(N)이다. 구현 상으로는 조금 더 복잡하겠지만 문제의 난이도가 크게 달라지지는 않는다.

# [코드](https://app.codility.com/demo/results/training9SVDNZ-6QE/)

```javascript
function solution(K, A) {
	let count = 0;
	let localSum = 0;
	for (let i = 0; i < A.length; i++) {
		localSum += A[i];
		if (localSum >= K) {
			count++;
			localSum = 0;
		}
	}

	return count;
}
```
