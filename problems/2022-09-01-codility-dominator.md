---
title: Codility, Dominator

date: 2022-9-1

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

Array가 주어졌을 때, Array가 50%를 초과하여 가지고 있는 값을 dominator라고 한다. dominator가 있는 경우 dominator의 index 중 아무거나 반환하고, 없을 경우 -1을 반환하라.

# 문제접근

1. 순환하면서 카운팅 하고, 카운팅 결과를 바탕으로 dominator가 있는지 확인하고, 있다면 해당 dominator의 index를 찾는 3가지 단계로 접근했다. 좀 더 효율적인 방법이 있지 않을까 하는 생각도 들었다. 하지만 3단계로 나뉘었어도 각 단계가 O(N)이었기 때문에 성능 상 문제가 되지 않을 것이라고 생각해서 그대로 코드를 짰다.

2. Stack을 사용하여 dominator를 찾는 방법도 있었다. stack에 같은 값이 오면 push하고 그렇지 않으면 pop을 해준다. dominator가 있는 경우에 stack에 반드시 dominator가 남는다. 따라서 해당 값이 실제 dominator인지 확인하여 맞을 경우 해당 인덱스를 반환한다.

# [코드](https://app.codility.com/demo/results/trainingACDNDR-FV3/)
## 카운팅 방식
```javascript
function solution(A) {
	const records = {};
	A.forEach((elem) => {
		if (records[elem] === undefined) {
			records[elem] = 1;
		} else {
			records[elem] += 1;
		}
	});

	let dominatorVal = null;
	Object.entries(records).forEach(([key, val]) => {
		if (val > A.length / 2) dominatorVal = key;
	});

	let dominatorIdx = -1;
	if (dominatorVal !== null) {
		dominatorIdx = A.findIndex((elem) => elem === Number(dominatorVal));
	}

	return dominatorIdx;
}
```

## 스텍활용
```javascript
function solution(A) {
	const stack = [];
	let dominatorIdx = -1;
	A.forEach((elem, idx) => {
		if (stack.length === 0 || stack[stack.length - 1] === elem) {
			stack.push(elem);
			dominatorIdx = idx;
		} else {
			stack.pop();
		}
	});

	if (stack.length === 0) return -1;

	const dominatorCandidate = stack[0];

	let dominatorCnt = 0;
	A.forEach((elem) => {
		if (elem === dominatorCandidate) dominatorCnt++;
	});

	return dominatorCnt > A.length / 2 ? dominatorIdx : -1;
}
```
