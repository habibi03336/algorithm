---
title: Codility, Ladder

date: 2022-9-12

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

높이가 N까지 있는 사다리가 있다. 사다리를 올라갈 때 1개 혹은 2개 씩 올라갈 수 있다. 이때 N까지 도달하는 경우의 수는 몇 개인가? N이 배열 A로 주어질 때 도달 가능한 경우의 수를 2^(같은 색인의 배열 B의의 요소) 나누어 나머지를 반환하라.

# 문제접근

경우의 수는 N이 증가함에 따라 피보나치 수열로 증가한다. 왜냐하면 N으로 가는 경우의 수는 (N-1일 때의 모든 경우에서 마지막에 1을 추가한 것) + (N-2일 때 모든 경우에서 뒤에 2를 추가한 것) 이기 때문이다.

따라서 피보나치 수열을 구하 듯이 경우의 수를 구하면서 모듈로 연산을 적용해 주면 된다.

# 시행착오

[실패한 풀이(37%)](https://app.codility.com/demo/results/training3CWUCT-T4F/)  
[javascript에서 number형](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number)은 C에서 double과 같은 데이터타입(IEEE 754 double)으로 나타낼 수 있는 최대값은 약 2^1024이다. 하지만 문제에서 피보나치 수열의 5만번 째 수까지 접근을 하고 이는 number 형이 나타낼 수 있는 최대 값을 넘어가서 문제를 발생시킨다.

[실패한 풀이(62%)](https://app.codility.com/demo/results/trainingJJ6GCZ-FFX/)
이러한 문제를 해결하기 위해 피보나치 수열을 계산하고 매번 modulo연산을 하도록 했다. 하지만 이렇게 하니 O(L\*\*2)가 되어서 timeout error가 발생했다.

# [코드](https://app.codility.com/demo/results/trainingB4N2KD-77G/)

피보나치 수열을 한 번만 계산하는 대신 조건에서 주어진 가장 큰 수로 modulo 연산을 하여 저장하도록 했다.

2의 지수 값으로 modulo연산을 하기 때문에 조건 상 가장 큰 2\*\*30으로 미리 modulo 연산을 하여 저장하여도 결과에는 지장이 없다.

```javascript
function solution(A, B) {
	const casesNum = Array(A.length + 1).fill(1);
	const biggestModuloNum = 2 ** 30;
	for (let i = 2; i < A.length + 1; i++) {
		casesNum[i] = (casesNum[i - 1] + casesNum[i - 2]) % biggestModuloNum;
	}

	const result = A.map((elem, idx) => {
		return casesNum[elem] % (2 ** B[idx]);
	});

	return result;
}
```
