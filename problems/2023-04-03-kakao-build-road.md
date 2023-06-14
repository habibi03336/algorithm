---
title: 카카오 코딩테스트, 경주로 만들기

date: 2023-4-3

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/67259)

크기가 n x n인 2차원 배열에 경주로를 만든다. 직선 경로를 만드는 것은 100의 비용이 들고, 코너를 만드는 것에는 500의 비용이 든다. 주어진 2차원 배열에 0이 있으면 길을 만들 수 있는 빈 공간이고, 1이 있으면 막혀있는 벽이다. (0,0)에서 (n-1, n-1)로 경주로를 만들 때 최소의 비용을 반환하라.

- n의 크기는 3이상 25이하이다.
- (0, 0)과 (n-1, n-1)은 항상 0이며, 도달 할 수 있는 경우만 입력으로 주어진다.

# 문제접근

1.  dfs로 탐색을 해나가면서, 해당 경로에 필요한 비용을 기준으로 답이 될 수 없는 경로를 제외해 풀어내려고 했다.

        dfs 엄청 많은 경우를 모두 탐색하기 때문에 적절한 조건으로 답이 될 수 없는 탐색은 바로 멈춰야한다.

2.  처음에는 각 지점에 도달하는 비용을 하나로 관리하여, 이미 있는 비용보다 낮은 경우에만 탐색을 계속하도록 하려했다. 하지만 이렇게 하면 아래 그림같은 경우에 문제가 생긴다.

<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/habibi03336/algorithm/master/assets/img/kakao-build-road.jpeg" alt="kakao-build-road"/>
</div>

3. 따라서 비용을 수직적으로 온 경우의 비용, 수평적으로 온 경우의 비용을 나누어서 관리하였다. 수직적으로 온 경우에는 수직 비용과, 수평적으로 온 경우는 수평 비용과 비교하여 탐색을 계속할지 결정한다.

# 코드

```java
import java.util.*;
class Solution {
    private int straightCost = 100;
    private int cornerCost = 600;
    private int[][][] visited;

    public int solution(int[][] board) {
        visited = new int[board.length][board[0].length][2];
        Arrays.stream(visited).forEach(
            row -> Arrays.stream(row).forEach(elem -> Arrays.fill(elem, Integer.MAX_VALUE))
        );
        dfs(board, 1, 0, 100, true);
        dfs(board, 0, 1, 100, false);
        int[] finalCost = visited[visited.length-1][visited[0].length-1];
        return Math.min(finalCost[0], finalCost[1]);
    }

    private void dfs(int[][] board, int i, int j, int cost, boolean isVerticalMove){
        if(i < 0 || j < 0 || i == visited.length || j == visited[0].length){
            return;
        }
        if(board[i][j] != 0){
            return;
        }
        if(isVerticalMove){
            if(cost > visited[i][j][0]){
                return;
            }
            visited[i][j][0] = cost;
        }
        if(!isVerticalMove){
            if(cost > visited[i][j][1]){
                return;
            }
            visited[i][j][1] = cost;
        }
        int verticalCost = isVerticalMove ? straightCost : cornerCost;
        int horizontalCost = isVerticalMove ? cornerCost: straightCost;
        dfs(board, i+1, j, cost + verticalCost, true);
        dfs(board, i-1, j, cost + verticalCost, true);
        dfs(board, i, j+1, cost + horizontalCost, false);
        dfs(board, i, j-1, cost + horizontalCost, false);
    }
}
```
