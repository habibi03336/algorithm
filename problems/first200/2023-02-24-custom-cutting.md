---
title: 커스텀 문제, 커팅식

date: 2023-2-24

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

지몽건물이 완공되어 커팅식이 열린다. 커팅식에 사용되는 끈의 길이는 m이다. n개의 조각으로 끈을 자르려고 한다. 한 조각의 길이를 k 이하로 하려고 할때, 끈을 커팅하는 모든 경우를 반환하라.

- 커팅하는 길이는 1이상의 정수이다.
- 가능한 경우가 없으면 0을 반환한다.
- 1 <= m,n <= 100,000

# 문제접근

1. 실패한 풀이: 경우의 수 문제로 접근했다. 전체 경우의 수에서 k 이하 조건을 만족하지 않는 경우를 뺴주는 방식으로 풀고자 했다.

- 상황이 단순하게 분류되지 않고, 매우 복잡해져서 풀이에 어려움을 겪었다.

2. 시뮬레이션을 활용해보고자 했다. 더 작은 문제로 쪼개서 생각해 줄 수 있었다. 첫 번째 칸이 1일 때부터 k일 때까지로 나눌 수 있다.

- solution(m, n, k) = solution(m-1, n-1, k) + solution(m-2, n-1, k) + ... + solution(m-k, n-1, k)이다.
- 겹치는 호출이 많아서, dp로 최적화를 해줄 수 있다.

# 코드

## DP 최적화

```python
dp = [[None for i in range(100000)] for j in range(100000)]
def solution(m, n, k):
    if m == n:
        return 1
    if m < n or m > n*k:
        return 0
    if dp[m][n] is not None:
        return dp[m][n]
    total_cases = 0
    first_block_size = 1
    while first_block_size <= k:
        total_cases += solution(m - first_block_size, n-1, k)
        first_block_size += 1
    dp[m][n] = total_cases
    return total_cases
```

## 최적화 없이

```python
def solution(m, n, k):
    if m == n:
        return 1
    if m < n or m > n*k:
        return 0
    total_cases = 0
    first_block_size = 1
    while first_block_size <= k:
        total_cases += solution(m - first_block_size, n-1, k)
        first_block_size += 1
    return total_cases
```
