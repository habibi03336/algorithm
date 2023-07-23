---
title: Codility, Peaks

date: 2022-9-4

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

배열에서 인접한 값보다 큰 값을 가진 요소를 peak이라고 한다. 배열을 일정한 간격으로 자를 때 각 부분에 하나 이상의 peak이 들어가게 하고자 한다. 길이가 N인 배열 A가 주어졌을 때 최대로 자를 수 있는 조각의 개수를 구하여라.

# 문제접근

1. 일정한 간격으로 자르기 위해서는 배열 길이의 약수로 잘라야 한다. 배열 길이의 약수 중에서 이분탐색을 하여 조건을 만족하는지 확인하는 방식으로 최대로 자를 수 있는 개수를 구하고자 했다. 하지만 이분탐색을 적용하기 어려운 문제였다. 왜냐하면 3개 구간으로 나누는 것은 안 되고 4개 구간으로 나누는 것은 되는 경우가 있을 수 있기 때문이다. 3구간이 안 될 때 반드시 더 적은 2 구간으로 나눠야 하는 것이 아니다.
    
		3구간은 안 되는데 4구간은 되는 case
		[0, 1, 0, 1, 0, 0, 0, 0, 1, 0, 1, 0]

2. 따라서 이분탐색이 아니라 A의 peek 개수에서부터 시작하여 하나씩 줄여가면서 모두 확인해 보는 방법을 적용해야한다.

# 이분탐색 시행착오

[실패한 풀이 (45%)](https://app.codility.com/demo/results/training7NKWKT-ASX/)

[실패한 풀이 (90%)](https://app.codility.com/demo/results/trainingJ9H4XK-V5F/)

# 검색

검색을 해보니 많은 사람들이 공통적으로 푼 방법이 있었다. peaks를 뽑고, 그 peaks 개수를 최대 나눌 수 있는 블록개수로 블록개수를 하나씩 줄여가며 가능한 블록개수인지 확인하는 방법이다.

peek의 인덱스를 블록의 크기로 나누어주면, peek이 몇 번째 블록에 들어가는지 바로 알 수 있다.

해당 블록개수로 나누는 것이 가능한지 알기 위해서 특정 블록 구간을 나타내는 배열을 만든다. 모든 peek을 고려했을 때 모든 블록에 peek이 하나 이상씩 들어간다면 바로 그 블록개수를 반환하면 된다. 

시간 복잡도는 O((peek개수)^2)이다.

# [코드](https://app.codility.com/demo/results/trainingMHWERQ-ZHA/)
```javascript
function solution(A) {
	const peaks = [];
	A.reduce((pre, cur, idx) => {
		if (pre < cur && idx + 1 < A.length && cur > A[idx + 1]) {
			peaks.push(idx);
		}
		return cur;
	}, Number.MAX_VALUE);

	if (peaks.length < 2) return peaks.length;

	for (let i = peaks.length; i >= 1; i--) {
		if (A.length % i !== 0) continue;
		let count = 0;
		const blocks = Array(i).fill(0);
		const blockLen = A.length / i;
		peaks.forEach((elem) => {
			const blockIdx = Math.floor(elem / blockLen);
			if (blocks[blockIdx] === 0) {
				blocks[blockIdx] = 1;
				count += 1;
			}
		});

		if (count === i) {
			return i;
		}
	}
}
```
