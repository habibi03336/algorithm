---
title: LeetCode, 495. Teemo Attacking

date: 2023-2-15

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/teemo-attacking/description/)

티모가 공격을 하면 상대는 양의 정수 duration만큼 독 공격을 받게된다. duration이 2이고 1 시간에 공격을 하면 1,2 시간에 독 공격을 받게 된다. 만약 독 공격을 받고 있는 도중에 다시 공격을 받게 되면 다시 공격을 받은 시점부터 다시 duration만큼 독 공격을 받게된다. 위의 예시에서 2에 공격을 다시 받으면 1,2,3 시간 동안에 독 공격을 받게 된다.

티모가 공격을 한 시점이 정수 배열 timeSeries로 주어지고 공격의 유효시간 duration이 주어 질 때, 독 공격에 데미지를 입는 총 시간을 반환하라.

# 문제접근

1. 여러 구간이 있는 경우 겹칠 때 겹치는 구간끼리 합치는 문제이다.

2. 두 개의 포인터를 활용하여, 겹치는 경우 끝나는 시점만 갱신하고 겹치지 않는 경우 앞선 공격에 대한 지속시간을 카운팅해준다. i시점에 i-1 공격까지의 지속시간을 더해주기 때문에 반복문 이후에 한 번 더 마지막 공격에 대한 지속시간을 더해주어야 한다.

# 코드

```java
class Solution {
    public int findPoisonedDuration(int[] timeSeries, int duration) {
        int totalSeconds = 0;
        int start = timeSeries[0];
        int end = timeSeries[0] + duration - 1;
        for(int i = 1; i < timeSeries.length; i += 1){
            int t = timeSeries[i];
            if(end < t){
                totalSeconds += end - start + 1;
                start = t;
            }
            end = t + duration - 1;
        }
        totalSeconds += end - start + 1;
        return totalSeconds;
    }
}
```
