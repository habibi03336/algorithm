---
title: Codility, Count Non Divisible

date: 2022-9-4

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

길이가 N인 array가 주어진다. 각 array의 요소를 나누었을 때 나누어 떨어지지 않는 다른 요소의 개수를 반환하라.  
(1 <= N <= 50,000, 0 < 각 요소(integer) < 2 \* N)

예를 들어 A = [1, 3, 4, 2, 6]이라고 하면,

1은 3,4,2,6으로 나누어 떨어지지 않는다 => 4개  
3은 4,2,6으로 나누어 떨어지지 않는다 => 3개  
4는 3,6으로 나누어 떨어지지 않는다 => 2개  
2는 3,4,6으로 나누어 떨어지지 않는다 => 3개  
6은 4로 나누어 떨어지지 않는다 => 1개

따라서 [4, 3, 2, 3, 1]을 반환한다.

# 문제접근

1. 각 요소는 1 ~ 100,000의 정수이다. array의 index를 통해서 각 숫자를 표현하고 몇 번 등장하는지 카운팅할 수 있다. (O(N))
2. 에라토스테네스의 채를 응용하여 어떤 수의 약수를 보다 빠르게 알 수 있다. (O(N\*log(log(N))))
3. 약수를 모두 구하고, 배열에 있는 약수의 개수를 더한다. 이를 전체 길이에서 빼주면 나누어 떨어지지 않는 요소의 개수를 알 수 있다. (O(N\*log(N)))

따라서 O(N\*log(N))으로 해결할 수 있을 것이다.

# 시행착오

[실패한 풀이 (33%)](https://app.codility.com/demo/results/trainingZTHR9J-26Y/)

접근한 방식으로 코드를 잘 짯지만 틀렸다. 약수를 구하는 방식에 문제가 있었다. 에라토스테네스의 채를 활용해 구한 소수를 통해서 잘 끼워맞추면 약수를 잘 구할 수 있을 것이라고 생각했는데 그것이 생각만큼 쉽지 않았다. 1부터 sqrt(elem)까지 반복문으로 약수를 구해주는 간단한 방법이 더 깔끔하다.

# [코드](https://app.codility.com/demo/results/trainingF4THDZ-GAP/)

```javascript
function solution(A) {
	const numberCount = Array(2 * A.length + 1).fill(0);
	A.forEach((elem) => {
		numberCount[elem] += 1;
	});

	const result = A.map((elem) => {
		let count = 0;
		for (let i = 1; i * i <= elem; i++) {
			if (elem % i === 0) {
				if (elem / i === i) {
					count += numberCount[i];
				} else {
					count += numberCount[i];
					count += numberCount[elem / i];
				}
			}
		}
		return A.length - count;
	});

	return result;
}
```
