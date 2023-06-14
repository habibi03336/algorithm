---
title: 백준, 2503. 숫자 야구

date: 2023-5-11

categories:
  - 문제풀이

tags:
---

# [문제요약](https://www.acmicpc.net/problem/2503)

3자리 숫자야구게임을 한다. 1에서 9까지의 자연수를 중복하지 않게 선택한다. 선택한 수들이 미리 정해진 세 숫자에 포함되면 볼이고, 위치까지 같으면 스트라이크이다. 예를 들어 미리 정해진 숫자가 3 7 8이고 선택한 수가 9 3 8이면 1스트라이크 1볼이다. 

N번의 게임진행 결과가 주어질 때, 정답으로 선택할 수 있는 남아있는 옵션의 개수를 반환하라.

- N은 1이상 100이하이다.

# 문제접근

1. 각 게임 결과에 따라서 가능한 옵션이 달라진다. 예를 들어 0볼 0스트라이크이면, 선택된 모든 숫자가 정답 숫자에서 제외되야한다. 만약 2볼 1스트라이크면 남은 경우의 수는 3가지로 줄어든다. 스트라이크인 숫자 1개만 특정하고, 남은 두 개의 위치를 바꾸면 정답이기 때문이다.

2. 처음 시작할 때 모든 경우를 가능하다고 보고, 게임을 한 번씩 진행함에 따라, 결과와 맞지 않는 경우를 제외해가면 된다. 한 번이라도 결과와 맞지 않은 경우는 idempotent하게 제외해가면 여태까지의 모든 게임의 결과와 대응하는 경우만 남게된다. 

        모든 경우를 대략 1,000개라고 하면, 각 게임에서 1,000번의 경우를 모두 확인한다. 
        게임은 최대 100번 한다고 했으니 100,000번 정도의 연산이 일어난다.


# 코드

```java
import java.io.IOException;
import java.util.Scanner;

public class bj_2503 {
    static int[][][] cases = new int[10][10][10];
    public static void main(String[] args) throws IOException {
        //input
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int[][] input = new int[n][5];
        for(int i = 0; i < n; i += 1){
            int call = scanner.nextInt();
            int s = scanner.nextInt();
            int b = scanner.nextInt();
            input[i][0] = call / 100;
            input[i][1] = call % 100 / 10;
            input[i][2] = call % 100 % 10;
            input[i][3] = s;
            input[i][4] = b;
        }

        //output
        System.out.println(Integer.toString(solution(input)));
    }

    public static void checkCase(int first, int second, int third, int strike, int ball){
        for(int t = 111; t < 999; t += 1){
            int i = t / 100;
            int j = t % 100 / 10;
            int k = t % 100 % 10;
            if(i == j || i == k || j == k || i == 0 || j == 0 || k == 0){
                continue;
            }
            int ballCount = 0;
            if(i == second || i == third){
                ballCount += 1;
            }
            if(j == first || j == third){
                ballCount += 1;
            }
            if(k == first || k == second){
                ballCount += 1;
            }
            int strikeCount = 0;
            if(i == first){
                strikeCount += 1;
            }
            if(j == second){
                strikeCount += 1;
            }
            if(k == third){
                strikeCount += 1;
            }
            if(ballCount != ball || strikeCount != strike){
                cases[i][j][k] = -1;
            }
        }

    }

    public static int solution(int[][] tries){
        for(int[] t : tries){
            checkCase(t[0], t[1], t[2], t[3], t[4]);
        }
        int cnt = 0;
        for(int t = 111; t < 999; t += 1) {
            int i = t / 100;
            int j = t % 100 / 10;
            int k = t % 100 % 10;
            if (i == j || i == k || j == k || i == 0 || j == 0 || k == 0) {
                continue;
            }
            if(cases[i][j][k] != -1){
                cnt += 1;
            }
        }
        return cnt;
    }
}
```
