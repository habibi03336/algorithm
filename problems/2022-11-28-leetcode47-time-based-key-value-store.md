---
title: LeetCode, Time Based Key-Value Store

date: 2022-11-28
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/time-based-key-value-store/)

Time based key value store를 구현하라. 이 스토리지는 key, value, timestamp 세 인자를 받아 정보를 저장한다. 정보를 꺼낼 때는 key와 timestamp를 인자로 받는다. 정보를 꺼낼 때는 인자로 전달된 timestamp 보다 작거나 같은 timestamp와 key에 대한 value를 반환한다. 만약 해당 키에 대해 더 작은 timestamp가 많으면 가장 가까운(가장 최신의) timestamp를 가지는 값을 반환한다.

- 조건에 맞는 value값이 없다면 빈 문자열을 반환한다.
- set에 주어지는 timestamp는 strictly increasing한다.
- set, get 호출은 총합 최대 2 \* 100,000번이다.

```javascript
// 예시
TimeMap timeMap = new TimeMap();
timeMap.set("foo", "bar", 1); // store the key "foo" and value "bar" along with timestamp = 1.
timeMap.get("foo", 1); // return "bar"
timeMap.get("foo", 3); // return "bar", since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 is "bar".
timeMap.set("foo", "bar2", 4); // store the key "foo" and value "bar2" along with timestamp = 4.
timeMap.get("foo", 4); // return "bar2"
timeMap.get("foo", 5); // return "bar2"
```

# 문제접근

1. 가장 간단한 방법으로 set에 대해 각 키에 대한 value를 timestamp와 함께 배열로 저장하고, get에 대하여 선형탐색하여 반환하는 것이다. set은 O(1), get은 O(N)의 시간 복잡도를 가진다. strictly increasing 조건이 있기 때문에 set이 O(1)이다.

2. 좀 더 효율화하면 위의 방식에서 get을 이진탐색으로 바꿔줄 수 있다. 이진탐색을 적용하면 get은 O(logN)의 시간 복잡도를 가진다.

# 코드

## 선형탐색 방식

447 ms 87 MB

```javascript
var TimeMap = function () {
	this.storage = {};
};

/**
 * @param {string} key
 * @param {string} value
 * @param {number} timestamp
 * @return {void}
 */
TimeMap.prototype.set = function (key, value, timestamp) {
	const newRecord = [timestamp, value];
	if (this.storage[key] === undefined) {
		this.storage[key] = [newRecord];
	} else {
		this.storage[key].push(newRecord);
	}
};

/**
 * @param {string} key
 * @param {number} timestamp
 * @return {string}
 */
TimeMap.prototype.get = function (key, timestamp) {
	if (this.storage[key] === undefined) return "";
	const records = this.storage[key];
	for (let i = records.length - 1; i > -1; i--) {
		if (records[i][0] <= timestamp) {
			return records[i][1];
		}
	}
	return "";
};
```

## 이진탐색 방식

527 ms 99.7 MB

```javascript
var TimeMap = function () {
	this.storage = {};
};

/**
 * @param {string} key
 * @param {string} value
 * @param {number} timestamp
 * @return {void}
 */
TimeMap.prototype.set = function (key, value, timestamp) {
	const newRecord = [timestamp, value];
	if (this.storage[key] === undefined) {
		this.storage[key] = [newRecord];
	} else {
		this.storage[key].push(newRecord);
	}
};

/**
 * @param {string} key
 * @param {number} timestamp
 * @return {string}
 */
TimeMap.prototype.get = function (key, timestamp) {
	if (this.storage[key] === undefined) return "";
	const records = this.storage[key];
	let lo = 0;
	let hi = records.length - 1;
	while (lo <= hi) {
		const mid = Math.floor((lo + hi) / 2);
		if (records[mid][0] <= timestamp) {
			if (
				(mid + 1 < records.length && timestamp < records[mid + 1][0]) ||
				mid + 1 >= records.length
			)
				return records[mid][1];
		}
		if (records[mid][0] < timestamp) {
			lo = mid + 1;
		}
		if (records[mid][0] > timestamp) {
			hi = mid - 1;
		}
	}
	return "";
};
```

리트코드 결과상으로는 선형탐색이 더 빨랐다. 테스트에 중복되는 키가 적거나, 테스트의 사이즈가 크지 않아서 그런 것 같다.
