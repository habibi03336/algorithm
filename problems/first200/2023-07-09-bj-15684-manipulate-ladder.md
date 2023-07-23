---
title: 백준, 15684. 사다리 조작

date: 2023-7-9

categories:
  - 문제풀이

tags:
---

# [문제요약](https://www.acmicpc.net/problem/15684)

사다리 게임이 주어진다. 사디리 게임의 세로 줄(폭)은 N개이고, 가로 줄(높이)은 H개이다. 또한 이미 이어진 다리의 개수는 M개이다.

사다리 게임에 다리 몇 개를 추가해서 i번 시작점은 i번 째 끝점으로 가도록 게임을 조작하려고 한다.

조건에 맞도록 사다리 게임을 조작하기 위해 추가해야하는 최소 다리 개수를 반환하라.

- N은 2 이상 10 이하, H는 1이상 30이하, M은 0이상 (N-1) \* H 이하 이다.
- 불가능 하거나 3개를 초과하는 다리가 추가되어야 하면 -1을 반환한다.
- 다리가 같은 높이에서 연속적으로 이어져 있을 수는 없다. (사다리 게임 규칙)

# 문제접근

1. 브루트 포스 방식으로 DFS 백트레킹으로 풀 수 있다고 생각했다. N\*H 개의 다리 후보 중에서 1개 뽑는 경우, 2개 뽑는 경우, 3개 뽑는 경우를 모두 고려해도 N과 H가 크지 않아서 경우가 그렇게 많지 않다. 450만개 정도의 경우의 수가 있다.

# 구현

1. dfs를 하여 백트레킹을 구현했기 때문에 기본적으로 모든 경우를 모두 탐색한다. 따라서 이미 가능하다고 확인한 다리 개수보다 작은 경우에만 재귀적인 호출을 한다.

2. 이미 3개의 다리를 추가했는데도 불가능한 경우에 재귀호출을 하지 않는다. 여기서 한 번만 더 호출하게 되도 경우의 수가 크게 늘어나기 때문에 정확하게 봐줘야 한다. 이 조건을 조금 느슨하게 반영했다가 시간초과 문제를 맞닥드렸다.

3. 이미 앞서서 골랐던 경우를 또 고르지 않기 위해서 startX (시작하는 세로 줄) 인자를 넘겨서 최적화를 해주었다.

# 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {
    public static int RIGHT = 1;
    public static int LEFT = 2;
    public static int NONE = 0;
    public static void main(String[] args) throws IOException {
        //input
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
        int H = Integer.parseInt(st.nextToken());

        int[][] board = new int[H][N];
        for(int i = 0; i < M; i += 1){
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            board[a-1][b-1] = RIGHT;
            board[a-1][b] = LEFT;
        }
        //output
        System.out.println(Integer.toString(solution(board)));
    }

    public static int count = Integer.MAX_VALUE;
    public static int solution(int[][] board){
        dfs(board, 0, 0);
        return count == Integer.MAX_VALUE ? -1 : count;
    }

    public static void dfs(int[][] board, int addedCount, int startX){
        if(addedCount >= count){
            return;
        }
        if(isItoI(board)){
            count = addedCount;
            return;
        }
        if(addedCount == 3){
            return;
        }
        for(int i = startX; i < board.length; i += 1){
            for(int j = 0; j < board[0].length-1; j += 1){
                if(board[i][j] != RIGHT && board[i][j] != LEFT && board[i][j+1] != RIGHT){
                    board[i][j] = RIGHT;
                    board[i][j+1] = LEFT;
                    dfs(board, addedCount + 1, i);
                    board[i][j] = NONE;
                    board[i][j+1] = NONE;
                }
            }
        }
    }

    public static boolean isItoI(int[][] board){
        for(int i = 0; i < board[0].length; i += 1){
            int destination = finalDestination(board, i);
            if(destination != i){
                return false;
            }
        }
        return true;
    }

    public static int finalDestination(int[][] board, int start){
        int startX = 0;
        int startY = start;
        while(startX < board.length){
            if(board[startX][startY] == RIGHT){
                startY += 1;
            } else if (board[startX][startY] == LEFT){
                startY -= 1;
            }
            startX += 1;
        }
        return startY;
    }
}
```
