---
title: LeetCode, 621. Task Scheduler

date: 2023-1-31

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/task-scheduler/description/)

처리해야 하는 작업의 종류가 들어있는 배열 tasks가 주어진다. 또한 양의 정수 n이 주어진다. 같은 종류의 task는 연속으로 실행될 수 없고 n 만큼의 간격을 두고 실행이 가능하다. 필요한 경우 유휴상태로 task 실행 가능 시점까지 기다려야 한다. 이 떄 작업을 마칠 수 있는 최소 시간을 반환하라.

- task.length의 범위 \[1, 10,000\]

# 문제접근

1.
task를 종류 별로 세어서 가장 큰 값을 찾는다. 가장 많은 task를 기준으로 블럭을 나눌 수 있다. 같은 종류를 실행하기 위해서는 적어도 n의 간격이 있어햐 한다. 따라서 한 블럭에 idle n개와 가장 많은 task 1개 해서 총 n+1의 시간이 필요하다.

- 다만, 마지막 블럭의 경우 idle이 꼭 없어도 된다. 마지막 블럭에서 실행될 task(=가장 많이 실행되는 종류의 task)를 따로 더해준다.

그렇게 가장 많이 실행해야하는 task를 기준으로 최소로 걸리는 시간을 구해줄 수 있다. 만약 전체 task의 개수가 이 보다 작으면, idle의 위치에 다른 task를 넣어주면된다. 만약 전체 task의 개수가 더 크면 idle을 모두 채우고 더 큰 간격으로 task를 배치할 수 있으므로 task의 길이 자체를 반환하면된다.

# 코드

```javascript
/**
 * @param {character[]} tasks
 * @param {number} n
 * @return {number}
 */
var leastInterval = function (tasks, n) {
	const counter = {};
	for (let i = 0; i < tasks.length; i += 1) {
		if (counter[tasks[i]] === undefined) {
			counter[tasks[i]] = 1;
		} else {
			counter[tasks[i]] += 1;
		}
	}
	const countValues = Object.values(counter);
	const maxCount = Math.max(...countValues);
	const c =
		(maxCount - 1) * (n + 1) +
		countValues.filter((elem) => elem === maxCount).length;
	return c < tasks.length ? tasks.length : c;
};
```
