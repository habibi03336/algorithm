---
title: Codility, Fib Frog

date: 2022-9-9

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

개구리가 강을 건너려고 한다. 강의 넓이는 N이다. 강에는 밟고 넘어갈 수 있는 나뭇잎이 있다. 나뭇잎이 있는 위치가 배열로 주어진다. 개구리가 피보나치 수만큼씩만 점프를 할 수 있다고 할 때, 최소 몇 번 점프를 하면 강을 넘어갈 수 있는가?

# 문제접근

1. N이 100,000으로 그 안에 속하는 피보나치 수가 많지 않다.
2. 첫 시작점에서 피보나치 수로 갈 수 있는 위치를 탐색하고, 그 중에 나뭇잎이 있는 곳을 새로운 시작점으로 저장한다.(O(log(N)) = fibo 개수)
3. 이를 반복하며 그 만큼 점프를 했을 때 강을 건너는 위치에 도달할 수 있는지 확인한다.(O(N))

예상 시간 복잡도 : O(N \* (log(N)^2))

# 시행착오

[실패한 풀이(83%)](https://app.codility.com/demo/results/trainingQ5XE6Z-FKY/)

performance 테스트에서 일부 실패했다. 목적지에 도달했는지 확인하는 부분을 개선했더니 시간 복잡도가 개선되었다.

[실패한 풀이(91%)](https://app.codility.com/demo/results/trainingG4RU7Y-7XX/)

cyclic test를 통과하지 못했다. 같은 자리를 중복해서 확인하는 경우가 많은 경우 performance가 좋지 않은 탓이었다. 따라서 checked array를 통해 이미 앞서서 도달한 경우 next step에서 제외하도록 코드를 추가했다.

# [코드](https://app.codility.com/demo/results/training485ZVB-3ED/)

```javascript
function solution(A) {
	const fiboRecord = [];
	let [a, b] = [1, 1];
	while (b <= A.length + 1) {
		fiboRecord.push(b);
		const temp = a;
		a = b;
		b = temp + b;
	}

	let cnt = 1;
	let starts = [-1];
	const checked = Array(A.length).fill(false);
	while (true) {
		if (starts.length === 0) return -1;
		const nextStep = [];
		while (starts.length !== 0) {
			const start = starts.pop();
			for (let i = 0; i < fiboRecord.length; i++) {
				const fibo = fiboRecord[i];
				if (start + fibo === A.length) return cnt;
				if (
					start + fibo < A.length &&
					A[start + fibo] === 1 &&
					!checked[start + fibo]
				) {
					checked[start + fibo] = true;
					nextStep.push(start + fibo);
				}
			}
		}
		cnt++;
		starts = nextStep;
	}
}
```
