---
title: Codility, Count Semiprimes

date: 2022-9-5

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

어떤 범위가 주어졌을 때(범위는 여러 개가 주어진다), 세미프라임(반소수)이 몇 개인지 카운트 하는 문제였다. 세미프라임이란 두 개의 소수의 곱으로 만들 수 있는 자연수를 의미한다. (추가로, 세미프라임이면 약수가 딱 한 쌍밖에 없다.)

- 범위의 상단 최댓값(N): 50,000
- 범위 입력의 개수(M): 1이상 30,000이하

# 문제접근

1. 매 번 주어진 범위에 대해서 일일이 확인하는 것은 비효율적이라고 생각했다. 범위가 최대 50,000이고, 범위의 개수가 30,000이기 때문에 각 수를 일일이 확인하는 방식으로 접근할 수 없었다. 따라서 누적적으로 몇 개의 세미프라임이 있는지를 먼저 알아두고, 이 누적된 값을 빼는 방식으로 해당 범위에 있는 세미 프라임의 개수를 알아내고자 했다.

2. 에라토스테네스의 채를 활용한다. 에라토스테네스의 체에 2부터 50,000까지 각 수의 약수 중에 가장 작은 소수를 저장해놓는다. 이렇게하면 소수인 경우에는 아무런 수가 저장되지 않는다.   

3. 2부터 50,000까지의 수를 에라토스테네스의 채에 저장된 값으로 나눈다. 나눈 결과값이 소수이면(에라토스테네스의 채 저장된 수가 없으면) 그 수는 세미프라임이다.

- 만약 어떤 수가 두 소수의 곱으로 만들어진다면, 그러한 약수는 딱 한 쌍 밖에 있을 수가 없다. 
- 에라토스테네스의 채를 만드는 것의 시간복잡도는 O(N\*log(log(N)))이다.


# [코드](https://app.codility.com/demo/results/trainingC38UYJ-BXC/)

```javascript
function solution(N, P, Q) {
	const primeFactor = Array(N + 1).fill(0);

	let i = 2;
	while (i * i <= N) {
		let k = i * i;
		while (k <= N) {
			if (primeFactor[k] === 0) primeFactor[k] = i;
			k += i;
		}
		i++;
	}

	const cumulativeCounts = Array(N + 1).fill(0);
	i = 1;
	let cumsum = 0;
	while (i <= N) {
		i++;
		if (primeFactor[i] !== 0) {
			let devided = i / primeFactor[i];
			if (primeFactor[devided] === 0) cumsum++;
		}
		cumulativeCounts[i] = cumsum;
	}

	const results = Array(P.length).fill(0);
	for (let i = 0; i < P.length; i++) {
		results[i] = cumulativeCounts[Q[i]] - cumulativeCounts[P[i] - 1];
	}

	return results;
}
```
