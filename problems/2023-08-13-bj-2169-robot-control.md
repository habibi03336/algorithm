---
title: 백준, 2169. 로봇 조종하기

date: 2023-7-23

categories:
  - 문제풀이

tags:
---

# [문제요약](https://www.acmicpc.net/problem/2169)

N x M 배열이 주어진다. (0,0)에서 출발하여 (N-1, M-1)으로 이동한다. 왼쪽, 오른쪽, 아래 3 방향으로 움직일 수 있다. 한 번 방문한 칸은 다시 가지 못한다.

각 칸에 정수가 적혀있다. N-1, M-1에 도착할 때까지 모든 수를 더 한다고 할 때 가능한 가장 큰 값을 반환하라.

- N, M은 1이상 1,000 이하이다.
- 각 칸에 적혀있는 정수의 크기는 절대값 100을 넘지 않는다.

# 문제접근

1. 위로 이동하지 못한다는 게 특이한 점이다. 위로 움직이지 못하기 때문에, 첫 줄의 각 칸에 도달하는 점수는 정해져있다. 0,0에서 오른쪽으로 움직이는 경우 밖에 없다.

2. 첫 줄의 점수가 정해져있다고 생각하니, 그 밑에 줄의 칸에 대해서도 도달하는 가장 큰 값이 위에 줄에 따라서 결정된다는 것을 직감할 수 있었다. 더 작은 문제의 값으로 큰 문제를 풀 수 있는 DP로 접근해야겠다고 생각했다.

3. i,j 칸에 도달하는 경우의 수는 위에서 바로 내려오거나, 오른쪽에서 오거나, 왼쪽에서 오거나 3가지 중에 하나이다.

4. 위에서 바로 내려올 때의 값, 왼쪽에서 온다 했을 때 가장 큰 값, 오른쪽에서 온다했을 때 가장 큰 값을 각각 구해준다. 세 값 중 더 큰 값이 최종적으로 해당 칸으로 접근하는 가장 큰 값이 된다.

# 구현

1. 왼쪽에서 올 떄의 간 칸의 최대값, 오른쪽에서 올 때의 간 칸의 최대값 두 경우 각각에 대해서 dp를 적용한다.

- 왼쪽에서 접근 시에 i,j 칸의 최대값은 `max((i,j-1) 까지의 최대 값, (i-1, j)까지의 최대 값) + 현재 칸의 점수` 이다.
- 결과적으로 일반적인 DP 격자 문제에서 DP를 왼쪽에서 오른쪽으로 이동하는 경우에 대해서 한 번, 오른쪽에서 왼쪽으로 이동하는 경우에 대해서 한 번 해주는 것과 같다.

# 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
        //input
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
        int[][] board = new int[N][M];

        for(int i = 0; i < N; i += 1){
            st = new StringTokenizer(br.readLine());
            for(int j = 0; j < M; j += 1){
                board[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        //output
        System.out.println(solution(board));
    }

    public static int solution(int[][] board){
        int width = board[0].length;
        int[] dp = new int[width];
        dp[0] = board[0][0];
        for(int i = 1; i < width; i += 1){
            dp[i] = dp[i-1] + board[0][i];
        }
        for(int i = 1; i < board.length; i += 1){
            int[] dp1 = new int[width];
            int[] dp2 = new int[width];
            dp1[0] = dp[0] + board[i][0];
            for(int j = 1; j < width; j += 1){
               dp1[j] = Math.max(dp1[j-1] + board[i][j], dp[j] + board[i][j]);
            }
            dp2[width-1] = dp[width-1] + board[i][width-1];
            for(int j = width-2; j >= 0; j -= 1){
                dp2[j] = Math.max(dp2[j+1] + board[i][j] , dp[j] + board[i][j]);
            }

            for(int j = 0; j < width; j += 1){
                dp[j] = Math.max(dp1[j], dp2[j]);
            }

        }
        return dp[width-1];
    }
}
```
