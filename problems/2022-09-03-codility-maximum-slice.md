---
title: Codility, Maximum slice

date: 2022-9-3

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

길이가 n인 array가 주어진다.  P, Q가 0 <= P <= Q < n인 정수 일 때, P부터 Q까지 array를 자른다. 이를 slice라고 부른다. slice 요소 합의 최대값을 반환하라.

# 문제접근

array를 순차적으로 봤을 때, 앞 선 총합이 양수이면 포함하는 것이 반드시 좋다. 또한 앞 선 총합이 음수인 경우 끊고 새로운 slice의 시작을 하는 것이 최대값을 만든다.
이 두 가지 전제에 따라서, 앞 선 총합이 양수이면 다음 요소의 값을 더해 최댓값을 갱신하고, 그렇지 않으면 0으로 새롭게 시작하여 최댓값을 만들어 나가는 방식으로 문제를 풀었다.

# [코드](https://app.codility.com/demo/results/training6MTS56-KHP/)

```javascript
function solution(A) {
	let maxSliceTotal = -Number.MAX_VALUE;
	let sum = 0;
	A.forEach((elem) => {
		sum += elem;
		if (sum > maxSliceTotal) maxSliceTotal = sum;
		if (sum < 0) sum = 0;
	});

	return maxSliceTotal;
}
```

---

카데인 알고리즘으로 다음과 같이 풀 수도 있다고 한다.

```javascript
function solution(A) {
	if (A.length === 1) return A[0];

	let localMaxSum = A[0];
	let globalMaxSum = A[0];

	for (let i = 1; i < A.length; i++) {
		localMaxSum = Math.max(A[i], localMaxSum + A[i]);
		globalMaxSum = Math.max(globalMaxSum, localMaxSum);
	}

	return globalMaxSum;
}
```

[출처](https://sustainable-dev.tistory.com/24)
