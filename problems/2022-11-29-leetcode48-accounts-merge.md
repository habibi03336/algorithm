---
title: LeetCode, Accounts Merge

date: 2022-11-29
categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/accounts-merge/)

길이가 N인 배열이 주어진다. 배열의 요소는 문자열 배열이다. 배열의 첫 번째 요소에는 사용자의 이름이 문자열로 들어있다. 그 이후 요소에는 해당 사용자의 1개 이상의 이메일(계정)이 문자열로 들어있다.

겹치는 이메일을 가지는 경우 같은 사용자로 판단한다. 이 때 같은 사용자일 경우 하나의 배열로 merge하여 반환하라.

- 각 요소 배열의 이메일은 오름차순으로 정렬되어야 한다.
- 겹치는 이메일을 가지는 경우 이름도 같다.
- 이름이 같아도 동일한 사용자는 아닐 수도 있다.

## 예시

> Input: accounts = \[["지훈","hajihun@mail.com","jnh03336@mail.com"],["지훈","hajihun@mail.com","actbest@mail.com"],["예지","yaegi@mail.com"],["지훈","kimjihun@mail.com"]\]

> Output: \[["지훈","hajihun@mail.com","jnh03336@mail.com","actbest@mail.com"],["예지","yaegi@mail.com"],["지훈","kimjihun@mail.com"]\]

# 문제접근

1. 각 배열의 원소를 독립적인 노드, 그리고 이메일을 edge라고 할 수 있다. 연결된 그래프가 하나의 사용자가 되고, 그 노드에 붙은 모든 edge가 이메일이 된다고 파악했다.

2. 각 노드가 속하는 배타적인 집합(사용자)를 찾는 것이기 때문에 분리집합이라고 생각했고, union find를 알고리즘을 적용하여 풀이하려고 했다.

# 구현 방식

1. 그래프를 간선 리스트로 표현하고, 같은 간선 리스트에 있는 노드(같은 이메일을 가지는 노드)끼리 union(같은 집합으로 넣어주는 개념)을 해준다. 간선 리스트의 첫 노드를 기준으로 union을 한다.

2. 간선 리스트의 이메일을 순회하면서 속하는 집합(사용자, rootNode)에 오름차순으로 넣어준다.

# 코드

```javascript
/**
 * @param {string[][]} accounts
 * @return {string[][]}
 */
var accountsMerge = function (accounts) {
	const nodes = Array(accounts.length)
		.fill()
		.map((_, idx) => idx);
	const edges = {};
	for (let i = 0; i < accounts.length; i++) {
		for (let j = 1; j < accounts[i].length; j++) {
			if (edges[accounts[i][j]] === undefined) {
				edges[accounts[i][j]] = [i];
			} else {
				edges[accounts[i][j]].push(i);
			}
		}
	}
	const union = (x, y) => {
		const rootX = find(x);
		const rootY = find(y);
		nodes[rootY] = rootX;
	};
	const find = (x) => {
		if (x === nodes[x]) return x;
		return find(nodes[x]);
	};
	Object.values(edges).forEach((edgeNodes) => {
		if (edgeNodes.length < 2) return;
		const firstNode = edgeNodes[0];
		for (let i = 1; i < edgeNodes.length; i++) {
			union(firstNode, edgeNodes[i]);
		}
	});
	const results = [];
	const graphs = Object.entries(edges);
	graphs.sort((a, b) => Number(a[0] > b[0]) - 0.5);
	graphs.forEach(([email, edgeNodes]) => {
		const rootNode = find(edgeNodes[0]);
		if (results[rootNode] === undefined) {
			results[rootNode] = [accounts[rootNode][0], email];
		} else {
			results[rootNode].push(email);
		}
	});
	return results.filter((elem) => elem !== undefined);
};
```
