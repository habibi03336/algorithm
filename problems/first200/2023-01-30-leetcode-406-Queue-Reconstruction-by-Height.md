---
title: LeetCode, 406. Queue Reconstruction by Height

date: 2023-1-30

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/queue-reconstruction-by-height/description/)

길이가 n인 배열이 주어진다. 배열의 요소에는 사람의 키 h와 k가 있다. k는 그 사람 앞에 그 사람 보다 키가 크거나 같은 사람의 수를 나타낸다.

h와 k 조건에 맞도록 배열을 재구성하여 반환하라.

- n의 범위: \[1, 2000\]
- 항상 적절히 재구성할 수 있다.

# 문제접근

1. 제일 작은 사람부터 세운다고 가정하면 문제가 단순화된다. 제일 작은 사람부터 세우면 앞에 있는 null은 모두 자신보다 크거나 같은 사람이 오는 것이 된다. 그 개수를 세어서 만족하는 위치에 넣어주면 된다.

- 다만, 조건을 만족하는 위치에 더 작은 키를 가지는 요소가 들어 있을 수 있다. 따라서 그 다음 null인 부분까지를 찾아서 넣어준다.(답이 항상 있다는 조건 때문에 그 다음 null을 찾기만하여 넣어주는 것이 가능하다.)

# 코드

```javascript
/**
 * @param {number[][]} people
 * @return {number[][]}
 */
var reconstructQueue = function (people) {
	people.sort((a, b) => a[0] - b[0]);
	const res = Array(people.length).fill(null);
	for (let i = 0; i < people.length; i += 1) {
		const p = people[i];
		let j = 0;
		let cnt = 0;
		while (cnt !== p[1]) {
			if (res[j] === null || res[j][0] === p[0]) cnt += 1;
			j += 1;
		}
		while (res[j] !== null) {
			j += 1;
		}
		res[j] = p;
	}
	return res;
};
```
