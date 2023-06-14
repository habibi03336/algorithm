---
title: LeetCode, 46. Permutations

date: 2022-11-25
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/permutations/)

정수가 담긴 길이 N의 array가 주어진다. array의 원소로 만들 수 있는 모든 순열의 경우를 이차원 배열로 반환하라.

- 모든 원소의 값은 unique하다.

# 문제접근

1. 모든 순열의 경우의 수를 만드는 문제이다. 시작에서부터 뻗어가면서 경우의 수를 만들어 간다고 볼 수 있기 때문에 recursion을 활용하는 것이 좋다. recursion의 힘으로 간단하게 조건을 바꿔주면서 경우의 수를 만들어 나갈 수 있다.

2. 유향 그래프 문제로 볼 수 있다. 모든 노드가 양방향으로 이어져있을 때, 각 노드를 한 번 씩 거쳐 그래프 전체를 탐색하는 모든 경로 (모든 해밀턴 경로)를 반환하는 문제라고 할 수 있다. dfs와 백트레킹을 활용하여 풀이한다.

<div style="text-align: center;">
    <img src="/assets/img/leetcode-46.jpeg" alt="leetcode-46"/>
</div>

2. 재귀함수 한 번에 O(N)의 시간복잡도를 가진다. 재귀함수는 총 N^N번(N \* (N-1) \* (N-2) \* ... \* 1) 호출된다. 따라서 총 시간 복잡도는 O(N^N)이다.

# 구현

1. 객체를 통해서 이미 접근된 노드를 다시 경유하지 않도록 할 수 있다.

2. recursion의 매개변수로 남아 있는 노드를 전달 할 수 있다. 이 때 for loop 안에서 pop하고 다시 unshift하는 방식을 통해서 배열의 복사 없이 임시적으로 이미 경유된 노드를 뺐다가 다시 넣어주었다.

# 코드

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permute = function (nums) {
	const results = [];
	const subResult = [];
	const picked = {};
	const findPermutations = () => {
		if (nums.length === subResult.length) {
			results.push([...subResult]);
			return;
		}
		nums.forEach((num) => {
			if (!picked[num]) {
				subResult.push(num);
				picked[num] = true;
				findPermutations();
				subResult.pop();
				picked[num] = false;
			}
		});
	};
	findPermutations();
	return results;
};
```

```javascript
var permute = function (nums) {
	const res = [];
	const permutation = [];
	const dfs = (elems) => {
		if (elems.length === 0) {
			res.push(permutation.slice());
			return;
		}
		for (let i = 0; i < elems.length; i++) {
			const tmp = elems.pop();
			permutation.push(tmp);
			dfs(elems);
			elems.unshift(tmp);
			permutation.pop();
		}
	};
	dfs(nums);
	return res;
};
```
