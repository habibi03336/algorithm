---
title: 연습문제, 하노이의 탑 푸는 프로그램

date: 2023-6-5

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제

하노이 탑 문제를 푸는 프로그램을 작성하라.

하노이 탑 문제는 원반을 옮기는 문제이다. 세 개의 기둥이 있고, 첫 번째 기둥에 크기가 서로 다른 원반이 끼워져있다. 아래에 있는 원판이 제일 크고 위로 갈 수록 작아진다. 이 원판을 세 번째 기둥으로 옮기면 된다. 다만 다음의 규칙에 따라 옮겨야한다.

1. 원반을 한 번에 하나만 옮긴다.
2. 맨 위에 있는 원반을 다른 기둥으로 옮길 수 있다.
3. 자신보다 작은 원반 위로 놓일 수는 없다. 

첫 기둥에 끼워져있는 원반의 개수 N이 주어질 때, 하노의 문제를 푸는 프로그램을 작성하라.

# 문제 접근

1. "N개 하노이 탑 문제"는 "N-1개 하노이 탑 문제"로 부터 풀릴 수 있다. "N-1 하노이 탑 문제"에서 N-1개의 원반을 두 번째 기둥으로 옮기고, 제일 큰 원반을 세 번째 기둥으로 옮긴다. 그렴 첫 번째 기둥이 비게 된다. 두 번째에 껴져있는 N-1개의 원반을 첫 기둥을 버퍼 삼아 세 번째 기둥으로 옮기면 된다. 

    즉, 1개 하노이 탑 문제를 풀 수 있으면, 2개 하노이 탑 문제를 풀 수 있고, 2개 하노이 탑 문제를 풀 수 있다.

    원반 1개를 옮기는 아주 아주 간단한 문제로부터 시작하여, 훨씬 더 복잡한 문제를 풀 수 있다. 또한 재귀를 활용하여 코드를 단순화할 수 있다.

# 코드
```python
class Tower:
    def __init__(self):
        self.stack = []

    def push_disk(self, disk):
        if not self.stack or self.stack[-1] > disk:
            self.stack.append(disk)
        else:
            raise RuntimeError("해당 기둥으로는 원반을 움직일 수 없습니다.")

    def move_top_to(self, destination):
        destination.push_disk(self.stack.pop())

    def move_n_disk_to(self, n, destination, buffer):
        if n == 1:
            self.move_top_to(destination)
            return

        self.move_n_disk_to(n-1, buffer, destination)
        self.move_top_to(destination)
        buffer.move_n_disk_to(n-1, destination, self)

    def __str__(self):
        return ",".join([str(elem) for elem in self.stack])


towers = [Tower() for i in range(3)]
N = int(input())
for i in range(N):
    towers[0].push_disk(N-i)

towers[0].move_n_disk_to(N, towers[2], towers[1])
print(towers[2])

```