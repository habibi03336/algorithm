---
title: LeetCode, 552. Student Attendance Record II

date: 2023-2-25

categories:
  - 문제풀이

tags:
  - Exercise
  - 🧑🏻‍💻
---

# [문제요약](https://leetcode.com/problems/student-attendance-record-ii/)

결석을 A, 지각을 L, 출석을 P로 표현한다. 통틀어 지각을 2회 미만으로 하고, 연속으로 3번 이상 지각한 적이 없으면 개근상을 받을 수 있다. n일 동안 수업이 있었다고 할 때, 개근상을 받을 수 있는 모든 경우의 수의 개수를 반환하라.

- 1 <= n <= 100,000
- 답이 매우 커질 수 있으므로 10^9 + 7로 모듈로 연산을 한 값을 반환하라.

# 문제접근

1. 문제를 더 작은 문제로 쪼개어 풀려고 했다. n일에 대해서 개근상을 받는 경우는 다음과 같이 쪼개진다.

- 1일에 결석하고 나머지 n-1일에 결석 한 번 도 안 하고 개근상 받는 조건 충족하기
- 1일에 지각하고 뒤 이은 2일에 대해서 연속으로 지각 안하고, 나머지 n-3일에 대해 개근상 받는 조건 충족하기
- 1일에 출석하고 뒤 이은 n-1일 대해 개근상 받는 조건 충족하기

2. 지각을 한 뒤의 경우가 다소 복잡하다. LLL이 나오면 안 되기 때문에, LA인 경우, LP인 경우, LLA인 경우, LLP인 경우를 더해서 경우의 수를 구해줄 수 있다. 엣지케이스로 n이 2 이하인 경우, LA, LP, LL (LLA, LLP가 아님) 이렇게 가능하기 때문에 이 부분도 신경 써줘야 한다.

3. 베이스 케이스(if문과 dp로 표현됨)는 다음과 같다.

- 결석 횟수 초과시 0 : 경우의 수 조건 만족 X
- 결석 남고, n이 1이면 3 : A, L, P 가능
- 결석 없고 n이 1이면 2: L, P 가능
- n이 0이면 1: 그 앞까지 선택한 한 가지 경우 가능 (선택 없이 끝내는 한 가지 경우 가능)
- n이 음수면 0: 길이 조건 만족 X

4. 결석 횟수와 n에 대해서 여러번 겹치는 호출이 있으므로 메모이제이션으로 효율화 해줄 수 있다.

# 구현

1. 기존 메쏘드에는 결석 가능일 수를 받는 매개변수가 없어서 해당 변수를 가지도록 함수 오버로딩을 하였다.

2. 조건상 모듈로 연산을 해줘야 하는데, 개수를 구할 때 빼주는 방식으로 해주면 모듈로 연산을 알맞게 적용해주기가 힘들다. 모듈로 연산은 다음과 같은 분배 법칙을 가지고 있다. 따라서 뺄셈을 쓰지않고 덧셈만을 활용해주는 것이 편하다.

> (A + B) % P = ((A % P) + (B % P)) % P

> (A - B) % P = ((A % P) - (B % P) + P) % P

# 코드

```java
class Solution {
    private char[] type = new char[]{'A', 'L', 'P'};
    private int[][] dp;
    private long moduloOperand = Math.round(Math.pow(10, 9) + 7);

    public int checkRecord(int n){
        dp = new int[2][n+1];
        for(int i = 0; i < dp.length; i += 1){
            Arrays.fill(dp[i], -1);
        }
        dp[0][1] = 2;
        dp[1][1] = 3;
        return checkRecord(n, 1);
    }

    public int checkRecord(int n, int absentLeft) {
        if(n < 0 || absentLeft < 0){
            return 0;
        }
        if(n == 0){
            return 1;
        }
        if(dp[absentLeft][n] != -1){
            return dp[absentLeft][n];
        }
        long totalCases = 0;
        for(char c : type){
            if(c == 'A'){
                totalCases += checkRecord(n-1, absentLeft - 1);
            }
            if(c == 'L'){
                totalCases += checkRecord(n-2, absentLeft);
                totalCases += checkRecord(n-2, absentLeft-1);
                if(n >= 3){
                    totalCases += checkRecord(n-3, absentLeft-1);
                    totalCases += checkRecord(n-3, absentLeft);
                } else {
                    totalCases += 1;
                }
            }
            if(c == 'P'){
                totalCases += checkRecord(n-1, absentLeft);
            }
        }
        dp[absentLeft][n] = (int) (totalCases % moduloOperand);
        return dp[absentLeft][n];
    }
}
```
