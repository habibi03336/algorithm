---
title: 커스텀, 배열 안에 약수와 배수 개수 구하기

date: 2025-3-3

categories:
  - 문제풀이

tags:
---

# 문제요약

1 이상 100,000이하의 정수로 이루어진 배열이 주어진다. i번 째 수에 대해서 자기 자신을 제외하고(i != j) i번 째 수의 배수이거나 약수인 요소의 개수를 센 결과를 반환하라.

**ex**
\[5, 2, 4, 3, 7\] => \[0, 1, 1, 0, 0\]
\[2, 4, 8, 16\] => \[3, 3, 3, 3\]

- 배열의 크기는 1 이상 100,000 이하이다.
- 정수의 크기는 1 이상 100,000 이하이다.

# 문제접근

1. 배열의 크기 상 n^2으로 할 경우 시간초과가 난다.

2. 각 숫자의 개수를 미리 세는 것을 O(n)으로 할 수 있다.

3. 약수는 sqrt까지만 반복하면서 모두 찾을 수 있다.

4. 같은 숫자에 대해서는 배수/약수 개수를 또 셀 필요가 없다. (2, 3과 같이 작은 수는 배수를 구하는데 많은 반복이 필요하지만 dp를 적용하면 성능문제가 줄어든다.)

# 코드

```python
def getCount(arr):
    max_n = 100000
    count_arr = [0] * (max_n + 1)

    # arr 배열에 있는 값들의 출현 횟수 기록
    for n in arr:
        count_arr[n] += 1

    # dp 배열로 계산된 값을 저장하여 중복 계산 방지
    dp = [None] * (max_n + 1)

    # 결과를 저장할 배열
    answer = [0] * len(arr)

    # 각 원소에 대해 계산
    for index, n in enumerate(arr):
        # dp에 값이 있으면 이미 계산된 값 사용
        if dp[n] is not None:
            answer[index] = dp[n]
            continue

        # n이 1인 경우는 모든 값에 대해 배수/약수 관계가 성립
        if n == 1:
            answer[index] = len(arr) - 1
            continue

        # 약수 개수 구하기
        for i in range(1, int(n**0.5) + 1):  # 1부터 n의 제곱근까지
            if n % i == 0:
                if i * i == n:
                    answer[index] += count_arr[i]
                else:
                    answer[index] += count_arr[i]
                    answer[index] += count_arr[n // i]

        # 자기 자신을 빼주기 (약수 개수 구할 때 더해짐)
        answer[index] -= 1

        # 배수 개수 구하기
        for multiple in range(2 * n, max_n + 1, n):
            answer[index] += count_arr[multiple]

        # dp에 계산된 값 저장
        dp[n] = answer[index]

    return answer
```
