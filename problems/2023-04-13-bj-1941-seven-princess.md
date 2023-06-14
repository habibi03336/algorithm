---
title: 백준, 소문난 칠공주

date: 2023-4-13

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://www.acmicpc.net/problem/1941)

5 x 5 이차원 배열이 주어진다. 각 배열의 요소는 true 아니면 false이다. 각 노드가 상하좌우로 인접할 때, 연결된 요소 7개를 선택한다. 이 때 7개의 요소 중 4개 이상이 true인 경우의 수를 반환하라.

# 문제접근

1. dfs로 7개가 연결된 경우를 만들어 조건을 만족하는지 확인해주려고 했다. 하지만 한 노드에서 갈라지는 경우도 서로 연결된 것이기 때문에 dfs로 연결된 경우만 만들기가 다소 어려워보였다.

2. 재귀를 활용하여 아무렇게나 7개의 위치를 뽑은 후에, 이 위치들이 연결된 것인지 bfs를 통해서 확인해 줄 수 있다. 아무 위치에서나 시작하여 그것과 인접한 위치 중에 뽑힌 위치가 있으면 방문 표시를 하고 큐에 넣어준다. 큐가 빌 때까지 반복했을 때 뽑힌 모든 위치가 방문되었다면 연결되어 있다고 할 수 있다.


# 코드

```python
from collections import deque

board = []
for i in range(5):
    board.append([i == "S" for i in input().strip()])

directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]


def is_connected(positions):
    visited = {}
    for pos in positions:
        visited[pos] = False
    que = deque()
    que.append(positions[0])
    visited[positions[0]] = True
    while que:
        for _ in range(len(que)):
            x, y = que.popleft()
            for direction in directions:
                nx, ny = x + direction[0], y + direction[1]
                if nx < 0 or ny < 0 or nx > 4 or ny > 4:
                    continue
                new_pos = (nx, ny)
                if new_pos in visited and not visited[new_pos]:
                    visited[new_pos] = True
                    que.append(new_pos)
    return 7 == sum(visited.values())


combination = []
princess_count = 0


def solution(start):
    global princess_count;
    if len(combination) == 7:
        count_s = 0
        for elem in combination:
            if board[elem[0]][elem[1]]:
                count_s += 1
        if count_s >= 4 and is_connected(combination):
            princess_count += 1
    else:
        for i in range(start, 25):
            combination.append((i // 5, i % 5))
            solution(i + 1)
            combination.pop()


solution(0)
print(princess_count)
```
