---
title: LeetCode, Majority Element

date: 2022-10-23
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/majority-element/)

정수 요소로 이루어진 길이가 n인 array가 주어진다. array에서 ⌊n / 2⌋를 초과하는 개수의 요소를 majority element라고 부른다. majority 요소가 무엇인지 반환하라.

- n은 1 ~ 50,000
- majority element는 항상 존재한다.

# 문제접근

1. sort를 한 후 (O(nlogn)) 직선적으로 각 요소가 몇 번 등장하는지 확인(O(n))해 줄 수 있다. sorting이 된 경우에는 바로 다음 요소와 비교만 해주면 되기 때문이다. 시간 복잡도 O(nlogn), 공간 복잡도 O(1)이다.

2. nums를 hash table을 활용하여 count해주는 방식도 있다. 시간 복잡도, 공간 복잡도 O(n)이다.

3. 분할 정복 방식으로 풀어 줄 수 있다. 배열을 두 부분으로 나누고, 각 부분에서 가장 많이 등장하는 요소를 하나 씩 반환 받는다. 그 다음에 두 요소 중에서 majority인 요소를 반환해주면 된다. O(nlogn) 공간복잡도 O(nlogn)이다.

- 분할 정복은 1) 간단한 문제 2) 분할 3) 정복 3단계로 나누어 재귀 코드를 작성할 수 있다.

# 코드

## sorting을 활요한 풀이

```javascript
var majorityElement = function (nums) {
	nums.sort();

	let pre = nums[0];
	let count = 0;
	const threshold = Math.floor(nums.length / 2);
	for (let i = 0; i < nums.length; i++) {
		const cur = nums[i];
		if (pre === cur) {
			count++;
		} else {
			pre = cur;
			count = 1;
		}
		if (count > threshold) return cur;
	}
};
```

다른 사람 풀이를 보고 ⌊n / 2⌋를 초과한다는 조건을 활용해 보다 간단하게 풀 수도 있다는 걸 발견했다. 전체 배열에서 차지하는 개수가 ⌊n / 2⌋를 초과하기 때문에 sorting을 하면 가운데를 항상 majority element가 차지 할 수 밖에 없다.

```javascript
var majorityElement = function (nums) {
	nums.sort();
	return nums[Math.floor(nums.length / 2)];
};
```

## hashtable을 이용한 카운팅 풀이

```javascript
var majorityElement = function (nums) {
	const numCount = {};
	const threshold = nums.length / 2;
	for (let i = 0; i < nums.length; i++) {
		if (numCount[nums[i]] === undefined) {
			numCount[nums[i]] = 1;
		} else {
			numCount[nums[i]]++;
		}

		if (numCount[nums[i]] > threshold) return nums[i];
	}
};
```

## 분할정복을 활용한 풀이

```javascript
var majorityElement = function (nums) {
	// 간단훈 문제
	if (nums.length === 0) return null;
	if (nums.length === 1) return nums[0];

	// 분할
	const half = (nums.length / 2) >> 0;
	const a = majorityElement(nums.slice(0, half));
	const b = majorityElement(nums.slice(half));

	// 정복
	const countA = nums.reduce((pre, cur) => {
		if (cur === a) pre += 1;
		return pre;
	}, 0);

	return countA > half ? a : b;
};
```
