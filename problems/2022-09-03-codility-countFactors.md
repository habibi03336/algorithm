---
title: Codility, Count Factors

date: 2022-9-3

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

어떤 수가 주어졌을 때 그 수의 약수의 개수를 반환하라.

# 문제접근

1. N의 범위가 1에서 2,147,483,647까지로 매우 컸다. 하지만 약수를 확인하기 위해서는 sqrt(N)까지만 loop을 돌면 되기 때문에 N이 최대인 2,147,483,647이라고 하더라도 46,340까지만 확인을 해보면 된다. 따라서 하나씩 일일이 확인해 보는 방식으로 접근할 수 있다.

2. 엣지케이스로 25(5*5)와 같이 약수가 홀수개인 경우를 고려해주어햐 한다. 

# [코드](https://app.codility.com/demo/results/trainingXPJ5N4-2NS/)

```javascript
function solution(N) {
	let count = 0;
	for (let i = 1; i < Math.floor(Math.sqrt(N)); i++) {
		if (N % i === 0) {
			count += 2;
		}
	}
	if (Math.floor(Math.sqrt(N)) ** 2 === N) count += 1;
	return count;
}
```

---
