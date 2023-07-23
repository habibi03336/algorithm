---
title: 백준, 2571. 세 수 고르기

date: 2023-4-5

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://www.acmicpc.net/problem/1503)

자연수 N이 주어진다. 자연수 3개 x, y, z를 곱하여(x\*y\*z) N과 가장 가까운 값을 만들려고 한다. 다만 x,y,z는 집합 S에 포함된 자연수를 취할 수 없다.
N과 N과 가장 가까운 x * y * z의 차를 반환하라.

- N은 1,000이하의 자연수 이다.
- 집합 S에는 최대 50개의 자연수가 있다. 집합의 요소는 1000이하의 자연수 이다.

# 문제접근

1. 브루트포스로 접근하는 경우 1,000^3 (10억)이라서 시간초과가 난다.

2. N에서 1씩 멀어지면서 그 수를 x\*y\*z로 만들 수 있는지 확인하는 방식을 떠올렸다. N이 최대 1000이고 집합 S의 크기가 50이기 때문에 적어도 플러스 마이너스 1,000 정도에는 답이 있지 않을까 하는 생각이 들었다. 

3. 그렇다면 각 숫자가 x,y,z로 나뉠 수 있는지를 확인하는 방식의 시간복잡도가 중요하다. 어떤 수 t가 있을 때 이 숫자가 3개로 나눠지는지 확인 하는 것은 일단 두 개로 나눈뒤, 두 개중 하나가 또 나뉠 수 있는지 확인하는 것과 같다. t가 두 개의 곱으로 나뉠 수 있는지 확인하는 것은 t의 약수를 확인하는 것과 같으므로 O(루트(t))이다. 그리고 t가 나뉜 약수 중 하나가 다시 나뉠 수 있는지 확인하는 것은 O(루트(t))보다는 작거나 같다. 숫자가 x,y,z로 나뉠 수 있는지를 확인하는 방식의 시간복잡도는 최대 O(t)이다.

4. N이 1000이라고 할 때, 플러스 마이너스 1000을 조회하면 대락 2000*1000번의 계산이 이루어진다.

# 코드

```python
N, _ = map(int, input().split())
s = set(map(int, input().split()))


def is_possible(num, t):
    if t == 1:
        if num in s:
            return False
        else:
            return True

    i = 0
    while i*i <= num:
        i += 1
        if i in s or num % i != 0:
            continue
        if is_possible(num // i, t - 1):
            return True
    return False


i = 0
while True:
    if (N - i > 0 and is_possible(N-i, 3)) or is_possible(N+i, 3):
        break
    i += 1

print(i)

```
