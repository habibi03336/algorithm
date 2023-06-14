---
title: Codility, Absolute Distinct

date: 2022-9-18

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

정수가 오름차순으로 들어있는 배열이 주어진다. 절댓값 기준으로 서로 다른 정수의 개수를 반환하라.

# 문제접근

1. 양수와 음수 중 같은 절대값을 가지는 수의 개수를 찾아내야 한다.

2. 양수와 음수를 가르는 인덱스를 찾은 후 음수 부분과 양수 부분을 차례로 비교해 나가면 O(N)으로 유니크한 절댓값의 개수를 알아낼 수 있다는 것을 발견했다.

# 시행착오

[실패한 풀이(35%)](https://app.codility.com/demo/results/training29628U-ZJG/)

non-decreasing 조건이기 때문에 중복되는 수가 들어있을 수 있는데 이 점을 간과했다. 사전에 유니크한 숫자만 뽑아내는 과정을 추가했다.

[실패한 풀이(92%)](https://app.codility.com/demo/results/trainingC9MRGZ-7RJ/)

한 문제에서 무한루프를 돌았다. 유니크한 숫자만 뽑아낸 배열을 기준으로 하도록 while문의 조건을 바꿔줬어야 했는데 실수로 바꾸지 않았다. 따라서 배열 길이 이상의 인덱스가 조회되어 무한 루프가 돌았다. 

다른 프로그래밍 언어에서는 인덱스 에러를 내겠지만 javascript에서는 이러한 경우 에러를 내지않고 undefined를 반환한다.

```javascript
const A = [1, 2, 3];
console.log(A[10000]); // undefined (no error)
```

# [코드](https://app.codility.com/demo/results/trainingXQ8PD7-6WA/)

```javascript
function solution(A) {
	const A_unique = [];
	A_unique.push(A[0]);
	for (let i = 1; i < A.length; i++) {
		if (A[i - 1] !== A[i]) {
			A_unique.push(A[i]);
		}
	}

	let splitIdx = -1;
	for (let i = 1; i < A_unique.length; i++) {
		if (A_unique[i - 1] < 0 && A_unique[i] >= 0) {
			splitIdx = i;
			break;
		}
	}
	if (splitIdx === -1) return A_unique.length;

	let i = 0;
	let j = 1;
	let count = 0;
	while (splitIdx - j >= 0 && splitIdx + i < A_unique.length) {
		const plusNum = A_unique[splitIdx + i];
		const minusNum = A_unique[splitIdx - j];

		if (plusNum + minusNum === 0) {
			count++;
			i++;
			j++;
		} else if (plusNum + minusNum < 0) {
			count++;
			i++;
		} else if (plusNum + minusNum > 0) {
			count++;
			j++;
		}
	}

	return count + (splitIdx - j + 1) + (A_unique.length - (splitIdx + i));
}
```
