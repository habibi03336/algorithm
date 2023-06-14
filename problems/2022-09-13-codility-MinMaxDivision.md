---
title: Codility, Min Max Division

date: 2022-9-13

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

길이가 N인 배열이 주어졌을 때, 이를 K개의 부분으로 나눈다고 하자. 이때 각 부분의 합 중 가장 큰 값을 large sum이라고 한다. 가장 작은 large sum의 값을 반환하라.

# 문제접근

1. 최대한 작은 large sum을 만들려고 할 때, large sum이 가질 수 있는 가장 큰 값은 (배열 값의 총합 / 나누는 부분의 수) + (요소 최대 값)이다. 이 값이 정답이 될 수 있는 값의 상한값이다.

   - 작은 large sum을 만들려고 할 때 최적은 총합을 정확히 K등분으로 나누는 것이다. 그러나 요소의 구성에 따라 총합을 정확히 K등분 하는 것은 어려운 경우가 많다. 하지만 그러한 경우에도 최대 요소의 최댓값까지만 벗어날 수 있다. 요소의 최댓값을 초과하여 벗어난다는 것은 이미 K등분을 정확히 한 만큼의 총합을 가지는데도 추가로 요소를 포함했다는 것으로 작은 large sum을 만드는 목적에서 벗어난다.

2. 이진탐색으로 0부터 상한값을 탐색하면서 정답을 찾아나간다. 후보가 되는 large sum이 주어졌을 때, 앞 쪽부터 더해나가면서 그리디하게 확인해 나가면 해당 large sum이 가능한 것인지 확인할 수 있다.

3. 이진탐색을 할 때 다소 애매한 지점이 있다. 예를 들어 large sum을 5이하로 만드는 것이 가능한데, 배열 상 실제 large sum이 5가 되도록 만드는 것은 불가능 할 수 있다. 5이하가 가능하기 때문에 5이하로 이진탐색을 계속해야한다. 

4. 이진탐색을 할 때는 주어진 수 이하로 가능한지만 확인을 하며 최소값을 찾은 다음에, 배열상 그 large sum을 만들 수 있는지는 추후에 확인한다. 만들 수 없다면 거기서부터 1씩 더해가면서 가능한 가장 작은 large sum을 찾아준다.


# 시행착오

[실패한 풀이(25%)](https://app.codility.com/demo/results/trainingQDHTGW-72X/)

문제접근 3번을 해결하기 위해 binary search의 방식을 조금 바꾸었다. 가능은 하지만 실제 array에서 만들 수 없는 경우 hi = hi + 2를 하여 하나 높은 것을 시도하도록 했다. 하지만 이 때무에 이진탐색이 무한루프를 돌았다. 이진탐색의 lo, hi 값을 추가적으로 바꾸는 것은 무한루프를 돌게할 위험이 크다.

[실패한 풀이(66%)](https://app.codility.com/demo/results/trainingF6QS88-J8B/)

무한루프를 간단히 해결하기 위해 이진탐색으로 가능한 최소값을 찾고, 만약 이것이 실제 값으로 나올 수 없다면 1씩 늘려가며 확인하도록 했다.

하지만 이번에는 isPossibleLargeSum 함수에서 문제를 발견했다. 만약 누적된 합이 아니라 해당 요소 자체가 기준이 되는 num보다 커버리면 함수가 예상하는 데로 작동하지 않았다.

이를 반영하여 코드를 수정해줬다.

# [코드](https://app.codility.com/demo/results/trainingVAUXKP-5V8/)

```javascript
function solution(K, M, A) {
	const total = A.reduce((pre, cur) => pre + cur, 0);
	const maxMinimalLargeSum = Math.floor(total / K) + M;

	const isPossibleLargeSum = (num) => {
		let sum = 0;
		let k = 1;
		let isExist = false;
		let isElemPossible = true;
		A.forEach((elem) => {
			if (elem > num) isElemPossible = false;
			if (sum + elem > num) {
				sum = elem;
				k++;
			} else {
				sum += elem;
			}
			if (sum === num) isExist = true;
		});

		if (k > K || !isElemPossible) return 0;
		if (!isExist) return 1;
		return 2;
	};

	let lo = 0;
	let hi = maxMinimalLargeSum;
	let result;
	while (lo <= hi) {
		const mid = Math.floor((lo + hi) / 2);
		const isPossible = isPossibleLargeSum(mid);
		if (isPossible === 2 || isPossible === 1) {
			result = mid;
			hi = mid - 1;
		} else if (isPossible === 0) {
			lo = mid + 1;
		}
	}


	while (isPossibleLargeSum(result) != 2) {
		result += 1;
	}

	return result;
}
```