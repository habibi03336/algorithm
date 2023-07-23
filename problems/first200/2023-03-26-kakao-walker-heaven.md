---
title: 카카오 코딩테스트, 보행자 천국

date: 2023-3-26

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/1832)

2차원 배열에서 제일 왼쪽 위에서 시작하여, 제일 오른쪽 아래로 이동하려고한다. 오른쪽 혹은 아래쪽으로 한 칸씩 이동이 가능하다. 

도로의 상황은 3가지로 나뉜다.
1. 0인 경우 오른쪽 혹은 아래쪽으로 자유롭게 이동할 수 있다.
2. 1인 경우 통행이 금지된 곳이다.
3. 2인 경우 회전이 금지되어있다. 왼쪽에서 온 경우 계속 오른쪽으로 가야하고, 위에서 온 경우 계속 아래쪽으로 가야한다.

배열의 크기를 나타내는 m,n과 도로의 상황을 나타내는 city_map이 주어질때, 제일 왼쪽 위에서 제일 오른쪽 아래로 가는 모든 경로의 수를 반환하라.

- 시작점 (0,0)과 도착지점 (m-1,n-1)의 도로 상황은 0이다.
- 경로의 수가 정수 범위를 넘을 수 있다. 따라서 20170805로 나눈 나머지를 반환하라.

# 문제접근

1. (a, b) 지점을 기준으로 봤을 때, (a, b) 지점에 도달하는 경우의 수는 ((a-1, b)에서 오는 경우) + ((a, b-1)에서 오는 경우)이다.

    - (a-1,b)의 도로상황이 1이라면 (a-1,b)에 위쪽에서 접근한 경로는 (a,b)로 올 수 없기 때문에 (a-1, b)의 왼쪽에서 접근한 경로만을 더해야 한다.
    - (a,b-1)의 도로상황이 1이라면 (a,b-1)에 왼쪽에서 접근한 경로는 (a,b)로 올 수 없기 때문에 (a, b-1)의 위쪽에서 접근한 경로만을 더해야 한다.
    - 만약 도로 상황이 0이라면 해당 경로로는 (a,b)에 올 수 없기 때문에 무시해야한다.

2. (a, b) 지점에 도달하는 경로의 경우의 수는 ((a-1, b)에서 오는 경우) + ((a, b-1)에서 오는 경우)라고 했다. 문제를 반복적으로 작게 쪼갤 수 있고, 앞서 푼 문제의 결과를 바타으로 더 큰 문제를 푸는 문제이다. 따라서 DP로 접근할 수 있다.

# 구현

1. 각 지점에서 왼쪽으로부터 온 경로와 위쪽으로부터 온 경로를 구분해야한다. 이를 따로 구분하기 위해 크기가 2인 정수배열을 활용했다.

2. dp 배열을 초기화 할 때 주의해야한다. 만약 중간에 도로상황이 1인 것이 있으면 그 뒤쪽으로도 모두 접근이 불가능하다.

3. MOD 식은 아래와 같은 분배법칙을 따른다. 분배를 해도 마지막에 한 번 더 연산을 해야한다. 따라서 정수의 범위를 넘지 않는 한에서 분배를 최소로 해주는 것이 좋다. 
각 지점에서 왼쪽에서 온 경로, 위쪽에서 온 경로의 개수를 모듈로 연산을 하여 관리했다. 이렇게 하면 최대 20,170,804*2까지만을 연산하게 하게 되므로 정수 범위에 문제가 없다.
> (a + b) % c = ((a % c) + (b % c)) % c

# 코드
```java
class Solution {
    int MOD = 20170805;
    public int solution(int m, int n, int[][] cityMap) {
        if(m == 1 && n == 1){
            return 1;
        }
        boolean isBlock = false;
        int[][] routeCases = new int[n][2];
        for(int i = 1; i < n; i += 1){
            isBlock = isBlock || cityMap[0][i] == 1;
            if(isBlock){
                routeCases[i] = new int[]{0, 0};
            } else {
                routeCases[i] = new int[]{1, 0};
            }
        }
        routeCases[0] = new int[]{0, 1};
        for(int i = 1; i < m; i += 1){
            if(cityMap[i][0] == 1){
                routeCases[0] = new int[]{0, 0};
            }
            for(int j = 1; j < n; j += 1){
                int[] cases = new int[]{0, 0};
                if(cityMap[i][j-1] == 0){
                    for(int c: routeCases[j-1]){
                        cases[0] += c;
                    }
                }
                if(cityMap[i][j-1] == 2){
                    cases[0] += routeCases[j-1][0];
                }
                if(cityMap[i-1][j] == 0){
                    for(int c: routeCases[j]){
                        cases[1] += c;
                    }
                }
                if(cityMap[i-1][j] == 2){
                    cases[1] += routeCases[j][1];
                }
                cases[0] = cases[0] % MOD;
                cases[1] = cases[1] % MOD;
                routeCases[j] = cases;
            }
        }
        
        int res = 0;
        for(int c: routeCases[n-1]){
            res += c;
        }
        return res % MOD;
    }
}
```