---
title: Codility, Min Abs Sum of Two

date: 2022-9-24

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

길이가 N인 배열이 주어진다. 두 요소 합의 절댓값 중 제일 작은 값을 반환하라.

- N은 100,000

# 문제접근

1. 크기 관계가 중요하다고 생각해서, 일단 정렬을 해주면 좋겠다고 생각했다.

2. 애벌레 방식으로 양 쪽에서 경우에 맞도록 좁혀가며 가장 작은 절댓값을 구할 수 있다. 다음 번에 lo+가 더 작은 값을 내는지, hi-가 더 작은 값을 내는지 비교해서 더 작은 쪽으로 이동하도록 하면된다.

3. 전체 시간복잡도는 O(N\*logN)을 기대할 수 있다.

# 애벌레(Caterpillar) 방식 합리화

lo요소, hi요소 모두 양수인 경우, hi가 - 하는 쪽으로만 움직이고, lo요소, hi요소 모두 음수인 경우 lo가 +하는 쪽으로만 이동한다.

lo요소는 음수, hi요소는 양수일 때는 둘의 합이 음인 경우는 lo + 쪽으로, 둘의 합의 양인 경우에는 hi - 쪽으로 이동한다.

헷갈리는 점이 있을 수 있다. 예를 들어 둘의 합이 음일 때 hi를 + 쪽으로 움직이는 것도 절댓값을 줄이는 방향이라고 여길 수 있다. 하지만 이는 이미 앞서서 고려된 절댓값이거나 최적화가 아닌 방식으로 움직이는 것이다.

<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/habibi03336/algorithm/master/assets/img/codility-minAbsSumOfTwo.jpeg" alt="codility-minAbsSumOfTwo" width="500"/>
</div>

# 시행착오

[실패한 풀이(54%)](https://app.codility.com/demo/results/trainingAECH92-93G/)

중복에서 같은 것을 선택할 수 있다는 점을 반영하지 않아서 틀렸다. 또한 lo+, hi- 방향을 정하는 조건을 보다 간단하게 할 수 있다.

# [코드](https://app.codility.com/demo/results/trainingEVNPKE-AHV/)

```javascript
function solution(A) {
	A.sort((a, b) => a - b);
	let result = Number.MAX_SAFE_INTEGER;
	let lo = 0;
	let hi = A.length - 1;
	while (lo <= hi) {
		const subResult = Math.abs(A[lo] + A[hi]);
		if (subResult < result) result = subResult;
		if (A[lo] + A[hi] < 0) lo++;
		else hi--;
	}

	return result;
}
```
