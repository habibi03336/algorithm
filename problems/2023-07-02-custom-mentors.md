---
title: 커스텀 문제, 상담 멘토

date: 2023-7-2

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

n명의 멘토가 있다. 또 k개의 상담유형이 있다. n명의 멘토를 하나의 상담유형에 배정하려고 한다. 각 상담유형에 적어도 한 명의 멘토를 배정해야한다고 할 때, 가능한 모든 멘토 배정결과와 그 경우의 수를 반환하라.

- k는 4이상 20 이하이다.
- n은 20이하, k이상의 정수이다.

# 테스트 케이스

    1. n = 3, k = 2
       => [[1, 2], [2, 1]], 2
    2. n = 5, k = 3
       => [
           [1, 1, 3],
           [1, 3, 1],
           [3, 1, 1],
           [1, 2, 2],
           [2, 1, 2],
           [2, 2, 1]
          ],
          6

# 문제접근

1. 재귀함수를 활용하여서 모든 경우의 수를 만들어 줄 수 있다.

2. 모든 경우를 만드는 시간복잡도는 중복조합으로, (n - k + 1) H (k-1)일 것이다. 일단 각 상담유형에 한 명씩 배정하고, 남은 n-k명을 줄세워 놓고 k-1개의 칸막이를 중복을 허용하여 치는 경우의 수이다.

   혹은 k개의 상담유형에 대해서 중복을 허용하여 n-k를 뽑는다고 볼 수도 있다. (k H n-k)

3. 중복조합은 다음과 같이 계산한다.

<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/habibi03336/algorithm/master/assets/img/repetition_combi.png" alt="cobination with repetition" width="300"/>
</div>

# 코드

```python
n = int(input())
k = int(input())

all_cases = []

category_mentors = [1 for i in range(k)]
def generate_cases(index, left):
    if left == 0:
        all_cases.append(category_mentors[:])
        return
    if index == len(category_mentors):
        return
    for i in range(left+1):
        category_mentors[index] += i
        generate_cases(index+1, left - i)
        category_mentors[index] -= i

generate_cases(0, n-k)

print(all_cases)
print(len(all_cases))
```

# 실험

얼마나 빠르게 경우의 수가 늘어날까?
k = 5로 고정되고 n이 달라질 때, n이 커짐에 따라서 급격하게 경우의 수가 늘어난다.

<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/habibi03336/algorithm/master/assets/img/repetition_combi_count.png" alt="cobination with repetition count fixed k" width="300"/>
</div>

n = 20으로 고정되고 k가 늘어날 때도, k가 커짐에 따라 급격하게 늘어난다.

<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/habibi03336/algorithm/master/assets/img/repetition_combi_count_2.png" alt="cobination with repetition count fixed n" width="300"/>
</div>
