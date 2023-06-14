---
title: Codility, Max Double Slice Sum

date: 2022-9-3

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

길이가 N인 array A가 주어진다. 0 <= X < Y < Z < N인 (X, Y, Z)를 double slice라고 한다. double slice의 합은 A[X+1] + A[X+2] + ... + A[Y-1] + A[Y+1] + ... A[Z-1]을 의미한다. 어떤 array A에 대해서 double slice의 최대 합을 구하여라.

# 문제접근

중간에 Y index 하나를 제외해야 한다는 점을 제외하면 maximum slice 문제와 같다. 최대 값을 내기 위해서는 가장 큰 음수 하나 혹은 가장 작은 양수 하나를 제외해야 한다.

# 시행착오

[실패한 풀이 1 (66%)](https://app.codility.com/demo/results/trainingKSZ9JP-MU5/)

처음에 접근할 때 가장 큰 음수를 제외할 수 있다고만 생각해서 틀렸다. 반드시 하나를 제외해야하는 것이고, 음수가 없는 경우 가장 작은 양수를 제외할 수도 있다는 점까지 고려해야했다.

[실패한 풀이 2 (92%)](https://app.codility.com/demo/results/trainingF5A4DX-YAQ/)

엣지케이스가 있는 거 같은데 잘 못찾겠다.

# 검색

[검색](https://sustainable-dev.tistory.com/25)을 해보니 대부분의 사람이 양방향 부분합이라는 개념을 통해서 문제를 접근한 것을 알 수 있었다.
[카데인 알고리즘](https://sustainable-dev.tistory.com/23?category=809125)이라는 방식을 응용한 것이다.

앞 부분에서부터 더 했을 때 최대 부분합을 저장하고, 뒷 부분에서부터 더 했을 떄의 최대 부분합을 저장한다.
그래고 각 index에 대해서 앞 뒤 부분 합을 더하고 이의 최대값을 구해주면 된다.

일반적인 부분합에서는 이렇게 해주지지만

> Math.max(A[i], sum[i-1] + A[i])

이 문제에서는 부분합이 음수인 경우 최대값을 구할 때는 0처리를 할 수 있기 때문에 이렇게 한다. ((3,4,7) 이런 식으로 앞 부분을 의도적으로 0으로 만들 수 있다.)

> Math.max(0, sum[i-1] + A[i])

# [코드](https://app.codility.com/demo/results/trainingJUDBM7-XF5/)

```javascript
function solution(A) {
	const headSum = A.map(() => 0);
	const tailSum = A.map(() => 0);

	for (let i = 1; i < A.length; i++) {
		headSum[i] = Math.max(0, headSum[i - 1] + A[i]);
	}

	for (let i = A.length - 2; i >= 0; i--) {
		tailSum[i] = Math.max(0, tailSum[i + 1] + A[i]);
	}

	let maxSum = 0;

	for (let i = 1; i < A.length - 1; i++) {
		const sum = headSum[i - 1] + tailSum[i + 1];
		if (maxSum < sum) maxSum = sum;
	}

	return maxSum;
}
```
