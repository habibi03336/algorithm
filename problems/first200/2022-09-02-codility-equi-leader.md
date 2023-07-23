---
title: Codility, EquiLeader

date: 2022-9-2

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

한 array에서 절반을 초과하는 개수를 가지는 값을 leader라고 한다. 예를 들어 array의 길이가 10일 때 어떤 값이 6개 이상 있으면 그 값을 leader라고 한다. 또한 Array를 어떤 index에 따라 쪼갰을 때 양쪽 모두에서 leader인 값이 있을 때 이를 equi leader라고 한다. 어떤 array가 주어졌을 때, equi leader가 존재하도록 하는 array의 index가 몇 개인지 반환하라.

# 문제접근

1. equi leader는 반드시 전체 array에 대해서도 leader이다. 따라서 equi leader가 존재하는지 알기 위해서는 leader가 있는지 부터 확인해야 한다고 생각했다. leader가 있는지 확인하고 없으면 바로 0을 반환한다.

2. leader가 있는 경우에는 leader 값의 개수를 모두 센다. 그리고 순차적으로 배열을 조회해가면서, 왼쪽 부분의 개수 leader의 개수, 오른쪽 부분 개수 leader의 개수를 업데이트한다. 각 인덱스에서 equi leader 조건을 충족하는지 확인해준다.

# [코드](https://app.codility.com/demo/results/trainingD3EWSG-M5S/)

```javascript
function solution(A) {
	const stack = [];
	A.forEach((elem) => {
		if (stack.length === 0 || stack[stack.length - 1] === elem) {
			stack.push(elem);
		} else {
			stack.pop();
		}
	});

	if (stack.length === 0) return 0;

	const leaderCandidate = stack[0];

	let totalLeaderNum = 0;
	const isLeaderArray = A.map((elem) => {
		if (elem === leaderCandidate) {
			totalLeaderNum++;
			return true;
		}
		return false;
	});

	if (totalLeaderNum < A.length / 2) return 0;

	let equiCount = 0;
	let rightLeaderCount = 0;
	let leftLeaderCount = totalLeaderNum;
	isLeaderArray.forEach((elem, idx) => {
		if (elem === true) {
			rightLeaderCount++;
			leftLeaderCount--;
		}

		if (
			rightLeaderCount / (idx + 1) > 0.5 &&
			leftLeaderCount / (A.length - (idx + 1)) > 0.5
		) {
			equiCount++;
		}
	});

	return equiCount;
}
```
