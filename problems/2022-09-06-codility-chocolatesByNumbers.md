---
title: Codility, Chocolates by numbers

date: 2022-9-6

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

초콜릿의 개수 N과 간격 M이 주어진다. 초콜릿을 둥그렇게 배치하고 M-1의 간격만큼을 지켜가면서 초콜릿을 먹는다. 예를 들어 N이 10이고 M이 4이면, 0번 4번 8번 2번 6번 이런 식으로 간격을 지켜가면서 초콜릿을 먹는다. 그러다가 이미 먹은 초콜릿을 만나면 그만 먹는다. 총 몇개의 초콜릿을 먹는지를 확인해라.

# 문제접근

최소공배수에 도달하면 처음 먹은 자리에 돌아오게된다. 그 때 까지 먹은 초콜릿의 개수를 구해주면 된다. 

최대공약수로 초콜릿의 개수 N을 나누면 원하는 답이 나온다. 최소공배수를 M으로 나눈 것과 최대공약수를 N으로 나눈 것의 값이 같기 때문이다.

	lcm = (a * b) / gcd

유클르디안 알고리즘으로 최대공약수를 구해준다.

# [코드](https://app.codility.com/demo/results/trainingS4QC8B-P26/)

```javascript
function solution(N, M) {
	let a = N;
	let b = M;
	while (a % b !== 0) {
		const temp = a;
		a = b;
		b = temp % b;
	}
	const gcd = b;

	return Math.floor(N / gcd);
}
```
