---
title: LeetCode, 239. Sliding Window Maximum

date: 2023-1-27

categories:
  - 문제풀이

tags:
  - Exercise
  - 🧑🏻‍💻
---

# [문제요약](https://leetcode.com/problems/sliding-window-maximum/description/)

정수가 담긴 배열 nums와 윈도우의 크기 k가 주어진다. k크기의 윈도우를 처음부터 끝까지 한 칸씩 옮겨가면서, 윈도우에서 가장 큰 값을 기록하여 배열로 반환하라.

# 문제접근

1. 윈도우에 숫자를 넣고 빼면서 매 윈도우에서 가장 큰 값을 찾아줄 수 있다. O(n\*k)의 복잡도를 가진다.

다음과 같이 추가적인 최적화를 해줄 수 있다.

- 가장 큰 값(currMax)을 따로 저장해두고, 새로 들어오는 수가 더 큰 경우에만 갱신을 해준다. O(1)
- 만약 window에서 나가는 값이 가장 큰 값(currMax)과 같은 경우 window에서 가장 큰 값을 다시 찾아준다. O(k)

2. 가장 큰 값을 index로 관리하면서, deque를 활용해 가장 큰 값을 효율적으로 업데이트할 수 있다. O(N)의 시간복잡도를 가진다.

- 1번 방식이 비효율적인 이유는, 윈도우가 옮겨질 때 O(k)로 가장 큰 값을 다시 찾아워야 하기 때문이다.
- 2번 방식이 효율적인 이유는, 윈도우가 옮겨질 때 가장 큰 값이 바뀌어야 한다면(가장 큰 값이 윈도우에서 나간다면) O(1)으로 가장 큰 값을 찾아 줄 수 있기 때문이다.
  - 가장 큰 값을 O(1)으로 찾을 수 있는 이유는, deque에 크기 순으로 index를 넣어놨기 때문이다.
  - 새로 들어오는 값 a가 deque에 있는 값 b보다 큰 경우, a가 window에 더 오래 남아 있으면서 더 큰 값이기 때문에 b는 더 이상 필요하지 않다. 따라서 빼줘도 된다.

# 코드

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var maxSlidingWindow = function (nums, k) {
	const res = [];
	const window = [];
	let currMax = -Number.MAX_SAFE_INTEGER;
	for (let i = 0; i < nums.length; i += 1) {
		window.push(nums[i]);
		if (i < k - 1) {
			continue;
		}
		if (currMax === -Number.MAX_SAFE_INTEGER) {
			currMax = window.reduce((pre, cur) => {
				if (pre > cur) return pre;
				return cur;
			}, -Number.MAX_SAFE_INTEGER);
		} else if (currMax < nums[i]) {
			currMax = nums[i];
		}
		res.push(currMax);
		if (currMax === window.shift()) {
			currMax = -Number.MAX_SAFE_INTEGER;
		}
	}
	return res;
};
```

```javascript
var maxSlidingWindow = function (nums, k) {
	const q = [],
		res = [];
	for (let i = 0; i < nums.length; i++) {
		while (q && nums[q[q.length - 1]] <= nums[i]) q.pop();
		q.push(i);
		if (q[0] === i - k) q.shift();
		if (i >= k - 1) res.push(nums[q[0]]);
	}
	return res;
};
```
