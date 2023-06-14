---
title: Codility, Nailing Planks

date: 2022-9-16

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

N개의 정수로 이루어진 배열 A, B가 주어진다. A\[i\] <= B\[i\]이고 각 인덱스는 A\[i\]부터 B\[i\]까지의 넓이를 가진 널판지를 의미한다. 또한 M개의 정수로 이루어진 배열 C가 주어진다. C의 각 요소는 못을 박는 위치를 의미한다. C에 주어진 순서대로 못을 박는다고 할 때 각 널판지에 적어도 하나의 못이 박히기 위해서 최소 몇 개를 박아야하는지 구하여라.

# 문제접근

1. 가능한 값의 하단과 상단이 주어진 상황이고, 선형적인 조건을 가지는 상황이므로 답을 이분탐색으로 구할 수 있다고 생각했다.

- 선형적인 조건: 선형적인 크기 관계를 통해 조건을 만족하는지 파악할 수 있는 상황(이 문제의 경우 10이 되면 반드시 11도 됨 or 7이 안 되면 반드시 5도 안됨)

2. 이분탐색시 mid값이 가능한지 파악하는 isPossible 함수를 효율적으로 짜는 것이 중요하다.

# 시행착오

[실패한 풀이(62%)](https://app.codility.com/demo/results/trainingEEA2BC-4HJ/)

딱히 좋은 방법이 생각나지 않아서 2차원 배열을 통해 각 위치에 있는 널판지를 모두 표시하고, 못의 위치를 조회해 가면서 못이 박힌 널판지를 체크해가는 방식으로 접근했다.

정확도 테스트는 통과했지만, 2차원 배열을 구성하는 것부터 O(N\*M)이라서 성능 테스트에서 시간초과가 났다.

# 검색

[검색 결과](https://codility-lessons.blogspot.com/2015/03/lesson-11-nailingplanks-nailing-planks.html) prefix sum을 활용하여 널판지가 너비에 못이 존재하는지 효율적으로 판단할 수 있다는 것을 알았다. 못의 위치를 표시하고, 이를 누적합 해준다. 그렇게 되면 두 위치 사이에 못이 있었다면 그 두 위치에 대한 누적합에 차이가 있게 되고 없었다면 누적합에 차이가 없게 된다. 이 문제의 경우 시작 위치와 끝나는 위치를 포함하여 (>= , <=) 못을 박을 수 있기 때문에 prefixSum\[끝나는위치\] - prefixSum\[시작위치 - 1\]로 해준다.

이를 활용하여 문제를 접근했다. isPossible 함수가 O(N+M)의 성능, binary search가 O(logM)의 성능이기 때문에 O((N+M) \* logM)으로 풀어낼 수 있었다.

# [코드](https://app.codility.com/demo/results/trainingQVCW2C-FZZ/)

```javascript
function solution(A, B, C) {
	const isPossible = (num) => {
		const prefixSum = Array(2 * C.length + 1).fill(0);
		for (let i = 0; i < num; i++) {
			prefixSum[C[i]]++;
		}

		for (let i = 1; i <= 2 * C.length; i++) {
			prefixSum[i] += prefixSum[i - 1];
		}

		for (let i = 0; i < A.length; i++) {
			if (prefixSum[B[i]] - prefixSum[A[i] - 1] === 0) return false;
		}

		return true;
	};

	let lo = 1;
	let hi = C.length;
	let result = -1;
	while (lo <= hi) {
		const mid = Math.floor((lo + hi) / 2);
		if (isPossible(mid)) {
			hi = mid - 1;
			result = mid;
		} else {
			lo = mid + 1;
		}
	}

	return result;
}
```
