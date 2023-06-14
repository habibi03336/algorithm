---
title: Codility, Max None Overlapping Segment

date: 2022-9-28

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

길이가 N인 두 배열 A, B가 주어진다. 각 인덱스의 A\[i\]는 segment의 시작, B\[i\]는 segment의 끝을 의미한다. J <= I 일 때 A\[J\] <= A\[I\] <= B\[J\] 이거나 A\[I\] <= B\[J\] <= B\[I\] 이면 overlapping이라고 한다. segment set을 만들 때 overlapping이 하나도 없는 경우를 None overlapping set이라고 한다. None overlapping set을 만들 때 최대로 있을 수 있는 segment의 개수는?

- N은 0 ~ 300,000
- J < I 일 때, B\[J\] <= B\[I\]
- segment는 끝나는 점 B\[i\]를 기준으로 오름차순 정렬되어있다.

# 문제접근

1. 그리디한 접근을 했다. 앞에서 부터 조회를 한다고 할 때, overlapping이 발생하면, 끝나는 점이 더 앞쪽인 것을 고르는 것이 최적이다.

- segement 끝나는 점을 기준으로 정렬되어있기 때문에 반대로 뒤에서부터 조회를 하면서, 시작 점이 더 뒤쪽인 것을 고르는 방식을 적용했다. 

# 시행착오

[실패한 풀이(20%)](https://app.codility.com/demo/results/trainingWAUGW7-V3Y/)

아이디어는 확실했는데 생각보다 구현이 바로 되지는 않았다. A.length를 A.lenght로 오타를 냈고, priorStart에 적절한 값을 넣지 못했다. 또 for문의 i값 설정이 좋지 못했다.

# [코드](https://app.codility.com/demo/results/training5ARR3N-9EK/)

```javascript
function solution(A, B) {
	if (A.length === 0) return 0;

	let count = 1;
	let priorStart = A[A.length - 1];
	for (let i = A.length - 2; i >= 0; i--) {
		if (priorStart > B[i]) {
			count++;
			priorStart = A[i];
		} else {
			priorStart = priorStart > A[i] ? priorStart : A[i];
		}
	}

	return count;
}
```
