---
title: LeetCode, Course Schedule

date: 2022-11-5
categories:
  - 문제풀이

tags:
  - Exercise
  - 🧑🏻‍💻
---

# [문제요약](https://leetcode.com/problems/course-schedule/)

강의의 개수 N이 주어진다. 그리고 선수과목들이 담긴 배열이 주어진다.  
선수과목은 길이가 2인 배열로 주어진다. 0번째 과목을 듣기 위해서는 배열의 1번째 과목을 들어야한다.

> 예를 들어 [0, 1]은 0 과목을 듣기 위해서는 1 과목을 들어야 함을 나타낸다.

선수과목 기준을 충족하면서 모든 과목을 들을 수 있는지 확인하라. 들을 수 있으면 true 없으면 false를 반환하라.

- N은 [1, 2,000]
- 선수과목 조건의 개수는 [0, 5000]
- 같이 선수과목의 조건이 중복해서 주어지지 않는다.

# 문제접근

1. 문제에서 요구하는 것은 선수과목이 서로 엉켜 a를 듣기 위해 b를 들어야하는데, b를 듣기 위해서는 a를 들어야하는 불가능한 상황이 있는지 확인하라는 것이다. 이는 결국 방향성이 있는 그래프에서 그래프가 cyclic 한 것인지를 확인하라는 의미이다. cyclic하다면 서로가 서로의 선수과목이 되어서 조건 충족이 불가능해진다. cyclic하지만 않다면 조건 충족에 문제가 없다.

2. [DFS 개념과 recursion을 활용하여 cyclic을 확인해 줄 수 있다.](https://www.geeksforgeeks.org/detect-cycle-in-a-graph/) 이미 cyclic의 부분이 아니라고 확인된 노드를 다시 탐색하는 비효율을 막기위해 visited 기록을 한다. 또 현재 탐색하고 있는 DFS에 노드가 포함되어 있는지 확인하기 위한 recursionStack을 활용한다. DFS를 구현하기 위해서 recursion을 활용한다. recursion을 통해 stack에 노드를 로직에 맞게 넣어줬다 뺴줬다 할 수 있다. recursion의 힘을 느낄 수 있었다.

3. 각 노드를 한 번씩 조회하기 때문에 시간복잡도가 O(N)이다.

# 코드

```javascript
/**
 * @param {number} numCourses
 * @param {number[][]} prerequisites
 * @return {boolean}
 */
var canFinish = function (numCourses, prerequisites) {
	const visited = Array(numCourses).fill(false);
	const recursionStack = Array(numCourses).fill(false);
	const graph = Array(numCourses)
		.fill()
		.map(() => []);
	prerequisites.forEach((requisite) => {
		graph[requisite[1]].push(requisite[0]);
	});

	const isCyclic = (node) => {
		if (recursionStack[node]) return true;
		if (visited[node]) return false;

		visited[node] = true;
		recursionStack[node] = true;

		for (const nextNode of graph[node]) {
			if (isCyclic(nextNode)) return true;
		}

		recursionStack[node] = false;

		return false;
	};

	for (let i = 0; i < visited.length; i++) {
		if (isCyclic(i)) return false;
	}

	return true;
};
```
