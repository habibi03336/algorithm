---
title: 연습문제, 행렬의 90도 회전

date: 2023-4-25

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

이차원 배열 M이 주어진다. 이차원 배열의 너비와 높이는 똑같이 N이다. 이차원 배열을 90도로 오른쪽으로 돌린다고 하자. 돌리기 전 (a, b)에 있던 요소가 들어가는 이차원 배열 상 위치는 어디인가?

# 문제접근

1. 이차원 배열을 시각적으로 나타내면 a는 행, b는 열을 나타낸다.

2. 90도로 오른쪽으로 돌리면 돌리기전 (a, b)는 (b, N - 1 - a)가 된다.

3. 추가로 90도로 왼쪽으로 돌리면 (a, b)는 (N - 1 - b, a)가 된다.

4. N=5인 예시는 다음과 같다.

<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/habibi03336/algorithm/master/assets/img/matrix-rotate.jpeg" alt="matrix-rotate" width="400"/>
</div>
