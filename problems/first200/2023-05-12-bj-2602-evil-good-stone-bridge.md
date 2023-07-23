---
title: 백준, 2602. 돌다리 건너기

date: 2023-5-12

categories:
  - 문제풀이

tags:
---

# [문제요약](https://www.acmicpc.net/problem/2602)
길이가 N으로 동일한 두 개의 돌다리 a,b가 주어진다. 각 돌에는 R, I, N, G, S 중 하나의 문자가 적혀있다. 

시작 지점에서 도착 지점으로 갈 때 순차적으로 밟아서 가야하는 문자열 s가 주어진다. (문자열은 R,I,N,G,S로만 이루어져있다.)

a,b 돌다리를 번갈아가며 밟아야하고, 반드시 한 칸 이상 앞으로 가야한다고 할때, 조건을 만족하며 시작점에서 끝점으로 가는 모든 경우의 수를 구하여라.

- N은 1이상 100이하
- s의 길이는 1이상 20이하


# 문제접근

1. DP로 접근하였다. 예를 들어, R,G,S를 밟고 도착까지 가는 경우는, R,G를 밟은 상태에서 그 이후에 S도 밟는 것이라고 할 수 있다. 큰 문제를 작은 문제로부터 풀어갈 수 있다.

2. 누적합 개념을 활용하였다. R,G를 밟아야 할 떄 경우의 수는, 각 G가 있는 위치 반대편 다리에 이전에 R을 밟은 경우의 수의 합이다. 각 경우의 수를 나타내주기 위해서 누적합 개념을 활용했다.


# 코드

```java
import java.io.IOException;
import java.util.Scanner;

public class bj_2602 {
    public static void main(String[] args) throws IOException {
        //input
        Scanner scanner = new Scanner(System.in);
        char[] steps = scanner.nextLine().toCharArray();
        char[][] bridges = new char[][]{ scanner.nextLine().toCharArray(), scanner.nextLine().toCharArray() };


        //output
        System.out.println(Integer.toString(solution(steps, bridges)));
    }

    public static int solution(char[] steps, char[][] bridges){
        int[][] dp = new int[2][bridges[0].length];
        for(int i = 0; i < 2; i += 1) {
            for(int j = 0; j < bridges[0].length; j += 1){
                if (bridges[i][j] == steps[0]){
                    dp[i][j] = 1;
                }
            }
        }
        int[][] prefixSumDp = generatePrefixSumDp(dp);

        for(int i = 1; i < steps.length; i += 1){
            clearDp(dp);
            for(int j = 0; j < 2; j += 1) {
                for(int k = 0; k < bridges[j].length; k += 1){
                    if (bridges[j][k] == steps[i]){
                        dp[j][k] = prefixSumDp[j==0?1:0][k];
                    }
                }
            }
            prefixSumDp = generatePrefixSumDp(dp);
        }

        return prefixSumDp[0][prefixSumDp[0].length-1] + prefixSumDp[1][prefixSumDp[1].length-1] + dp[0][dp[0].length-1] + dp[1][dp[1].length-1];
    }

    public static int[][] generatePrefixSumDp(int[][] dp){
        int[][] prefixDp = new int[dp.length][dp[0].length];
        for(int i = 0; i < dp.length; i += 1){
            int prefix = 0;
            for(int j = 0; j < dp[i].length; j += 1){
                prefixDp[i][j] = prefix;
                prefix += dp[i][j];
            }
        }
        return prefixDp;
    }

    public static void clearDp(int[][] dp){
        for(int i = 0; i < dp.length; i += 1){
            for(int j = 0; j < dp[i].length; j += 1){
                dp[i][j] = 0;
            }
        }
    }
}
```
