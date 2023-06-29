---
title: 커스텀 문제, 배열을 t개로 고르게 나누기

date: 2023-6-29

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

길이가 n인 배열이 있다. 이 배열의 요소를 t개의 배열에 나누려고 한다. 가장 고르게 n개를 분배할 때, t개 배열의 크기를 반환하라.

- t개 배열의 크기는 내림차순하여 반환한다.

# 테스트 케이스

    1. n = 5, t = 10
       => [1, 1, 1, 1, 1, 0, 0, 0, 0, 0]

    2. n = 30, t = 10
       => [3, 3, 3, 3, 3, 3, 3, 3, 3, 3]

    3. n = 111, t = 9
       => [13, 13, 13, 12, 12, 12, 12, 12, 12]

# 문제접근

1. n을 t로 나눈 몫만큼 각 배열의 크기로 배정한다. 그리고 나머지만큼 앞에서부터 개수를 추가해준다.

# 코드

```python
n = int(input())
t = int(input())

share = n // t
remain = n % t

sizes = [share for i in range(t)]
for i in range(remain):
  sizes[i] += 1

print(sizes)
```
