---
title: Codility, Flags

date: 2022-9-3

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

산의 높이를 기록한 array가 주어진다. 양쪽으로 인접한 값보다 큰 값을 가지는 경우 peak이라고 한다. peak에 깃발을 꼽을 수 있다. 다만 깃발을 가져가는 개수만큼 깃발의 거리가 멀어야 한다. 예를 들어 만약 4개의 깃발을 가져가면 깃발 간의 거리가 4 이상이어야 한다. 최대한 많은 깃발을 꼽는다고 할 때 그 개수를 반환하라.

# 문제접근

peak의 위치가 중요 정보이므로 peak의 위치를 먼저 뽑아냈다. 깃발을 1개 가져갈 때, 2개 가져갈 때 ... 식으로 깃발을 가져가는 갯수를 늘려가며 몇개까지 꼽는지 확인한다. 만약 3개 가져갔을 때와 4개 가져갔을 때 똑같은 개수만을 꼽을 수 있다면 그 개수가 최대인 것으로 보고 확인을 멈춘다.

- 왼쪽에서부터 가능한 한 순차적으로 꼽는 것이 '항상 최대로 꼽는' 하나의 경우이다.

# 시행착오

[실패한 풀이 (93%)](https://app.codility.com/demo/results/trainingZEJSR6-RAQ/)

timeout 에러가 났다. 내 풀이에서 peak이 많아 질 수록 하위루프가 많이 돌고, 상위루프가 적게 돈다. 따라서 이중루프라고해서 O(N^2)은 아니다. 그럼에도 성능이 충분하지 않았다.

```javascript
function solution(A) {
	const peaks = [];
	A.reduce((pre, cur, idx) => {
		if (pre < cur && idx + 1 < A.length && cur > A[idx + 1]) {
			peaks.push(idx);
		}
		return cur;
	}, Number.MAX_VALUE);

	let maxFlagNum = 0;
	for (let i = 1; i <= peaks.length; i++) {
		let dist = 0;
		let flagNum = 0;
		peaks.forEach((elem, idx) => {
			if (flagNum === i) return;

			if (idx === 0) {
				flagNum++;
				return;
			}

			dist += elem - peaks[idx - 1];
			if (dist >= i) {
				flagNum++;
				dist = 0;
			}
		});

		if(maxFlagNum >= flagNum){
			break;
		} else {
			maxFlagNum = flagNum;
		}
	}
	return maxFlagNum;
}
```

---

# 검색

[이분탐색을 활용해 문제를 푼 것을 발견했다.](https://coder-in-war.tistory.com/entry/Codility-Lesson10Medium-Flags)  
이 문제에서 시간복잡도 문제를 해결하기 위해서는 시도해보는 flag의 개수를 최소화 하는 것이 중요한데, 이를 이분탐색을 통해 해결했다. 좋은 접근이라 생각해 나도 적용해 보았다.

# [코드](https://app.codility.com/demo/results/trainingW43DM8-ZDA/)

```javascript
function solution(A) {
	const peaks = [];
	A.reduce((pre, cur, idx) => {
		if (pre < cur && idx + 1 < A.length && cur > A[idx + 1]) {
			peaks.push(idx);
		}
		return cur;
	}, Number.MAX_VALUE);

	function count(flagsBring) {
		let dist = 0;
		let flagNum = 0;
		peaks.forEach((elem, idx) => {
			if (flagNum === flagsBring) return;

			if (idx === 0) {
				flagNum++;
				return;
			}

			dist += elem - peaks[idx - 1];
			if (dist >= flagsBring) {
				flagNum++;
				dist = 0;
			}
		});

		return flagNum;
	}

	let lo = 0;
	let hi = peaks.length;
	let maxFlagNum = 0;
	if (hi < 2) return hi;

	while (lo <= hi) {
		let mid = Math.floor((lo + hi) / 2);
		if (count(mid) === mid) {
			lo = mid + 1;
			maxFlagNum = mid;
		} else {
			hi = mid - 1;
		}
	}

	return maxFlagNum;
}
```
