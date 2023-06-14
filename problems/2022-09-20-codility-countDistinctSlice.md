---
title: Codility, Count Distinct Slices

date: 2022-9-20

categories:
  - 문제풀이
  - 🧑🏻‍💻

tags:
  - Exercise
---

# 문제요약

길이가 N인 배열이 주어진다. 배열을 잘라서 그 안에 요소가 중복된 것이 없으면 "distinct slice"라고 한다. 각 요소는 0이상 M이하이다. M 그리고 길이가 N인 배열 A가 주어 질 때 distinct slice의 총 개수를 반환하라.

# 문제접근

1. 배열 A의 distinct slice 요소 개수는, (배열 A의 마지막 요소를 제외한 distinct slice 개수) + (마지막 요소로 추가되는 distinct slice 개수)이다.

2. 마지막 요소가 slice에 추가되고 중복이 아니라면, (기존 요소의 개수 + 1)만큼의 추가적인 distinct slice가 생긴다.

		예를 들어, [1, 2, 3]에 4가 추가되어 [1, 2, 3, 4]가 될 때 
		[1, 2, 3, 4], [2, 3, 4], [3, 4], [4] 이렇게 4개의 distinct slice가 추가된다.

3. 마지막 요소가 slice에 추가되는데 중복이라면, 중복이 없어지는 이후부터 + 1 만큼의 추가적인 distinct slice가 생긴다.

		예를 들어, [1, 2, 3]에 1가 추가되어 [1, 2, 3, 1]가 될 때 
		[2, 3, 1], [3, 1], [1]  이렇게 3개의 distinct slice가 추가된다.


4. 애벌레 방식으로 구현을 할 수 있다. O(N)의 시간복잡도를 기대할 수 있다.


# [코드](https://app.codility.com/demo/results/trainingSN4Z4P-V84/)

```javascript
function solution(M, A) {
	const checked = Array(M + 1).fill(false);

	let right = 0;
	let cnt = 0;
	for (let left = 0; left < A.length; left++) {
		while (right < A.length && !checked[A[right]]) {
			cnt += right - left + 1;
			checked[A[right]] = true;
			right++;
		}
		checked[A[left]] = false;
		if (cnt > 1000000000) return 1000000000;
	}

	return cnt;
}
```
