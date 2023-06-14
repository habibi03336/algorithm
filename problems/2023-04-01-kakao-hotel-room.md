---
title: 카카오 코딩테스트, 호텔 방 배정

date: 2023-4-1

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/64063)

1부터 k까지 번호가 붙은 호텔방을 배정한다. 만약 손님이 원하는 방이 비어있으면 바로 배정하고, 그렇지 않으면 그보다 높은 번호의 방 중 가장 작은 번호의 방을 배정한다. 손님이 원하는 방의 번호가 순서대로 크기가 n인 배열로 주어질 때, 방이 배정된 결과를 반환하라.

- k는 1이상 10^12(1조) 이하인 자연수
- n은 1이상 200,000이하의 자연수
- 손님은 1이상 k이하의 존재하는 방에 대해서만 요청한다.
- 손님이 방을 배정받지 못하는 경우는 입력으로 주어지지 않는다. (ex: k=5, [5, 5])

# 문제접근

1. 해당 칸이 이미 차있으면 다음 비어있는 칸에 배정하는 것이 해쉬테이블 충돌 해결 방법 중에 하나인 선형 프로빙과 굉장히 닮은 문제이다.

2. 해당 칸이 차있을 때 다음 칸을 찾아내는 방식을 최적화해야한다. 그렇지 않으면 최악의 경우 O(n^2)이고 문제 조건 상 시간초과가 날 것이다.

3. 첫 번째 접근으로, 해당 칸이 앞서서 몇 번 조회됐는지를 카운팅하고, 그 카운팅 결과를 바탕으로 다음 칸을 구해주는 방식으로 최적화를 했다. 하지만 효율성에서 일부 시간초과가났다.

        만약 t번 칸이 차있는데 추가로 3번 조회했다면 
        적어도 t+1, t+2, t+3은 차있다는 의미이기 때문에 3칸을 건너뛰고 t+4가 비어있는지 확인하면 된다.

4. 연속적으로 배정된 방들이 동일한 객체 하나를 가르키게 하고, 그 객체가 다음으로 가능한 방 번호를 상태로서 관리하게하면 O(1)으로 바로 다음 방을 찾을 수 있다. 따라서 O(n)으로 풀 수 있다. 

    하지만 연속적으로 배정된 방이 동일한 객체를 가르키게 하는 것은 어려웠다. 왜냐하면 원래 분리 되어 있다가 이어지는 경우가 있었기 때문이다. 말 그대로 동일한 객체를 가르키게 하기 위해서는 여러 칸에 대한 업데이트가 필요했다. 따라서 이러한 경우 업데이트가 필요한 객체가 next로 다음 상태를 가르키게하고 유니온 파인드의 개념처럼 재귀적으로 마지막 객체에 도달하게 하였다.

    이 방식을 사용하기 위해서는 객체를 어떻게 배정하고 엮을지에 대해서 경우에 따라 분기하여 처리해주어야 했다.


# 코드

## 첫 번째 접근, 일부 시간초과
```java
import java.util.*;

class Solution {
    public long[] solution(long k, long[] room_number) {
        Map<Long, Integer> counts = new HashMap<>();
        long[] result = new long[room_number.length];
        for(int i = 0; i < room_number.length; i += 1){
            long room_idx = room_number[i];
            while(counts.containsKey(room_idx)){
                int jumpCount = counts.get(room_idx);
                counts.put(room_idx, jumpCount + 1);
                room_idx += jumpCount + 1;
            }
            counts.put(room_idx, 0);
            result[i] = room_idx;
        }
        return result;
    }
}
```

## 두 번째 접근, 성공
```java
import java.util.*;

class NextRoomState {
    public NextRoomState next = null;
    public long val;
    public NextRoomState(long nrn){
        val = nrn;
    }
    public NextRoomState increase(){
        val += 1;
        return this;
    }
    public NextRoomState getFinalState(){
        NextRoomState nrs = this;
        while(nrs.next != null){
            nrs = nrs.next;
        }
        return nrs;
    }
}

class Solution {
    public long[] solution(long k, long[] room_number) {
        Map<Long, NextRoomState> counts = new HashMap<>();
        long[] result = new long[room_number.length];
        for(int i = 0; i < room_number.length; i += 1){
            long room_idx = room_number[i];
            if(counts.containsKey(room_idx)){
                NextRoomState nextRoomState = counts.get(room_idx).getFinalState();
                result[i] = nextRoomState.val;
                if(counts.containsKey(result[i]+1)){
                    counts.put(result[i], counts.get(result[i]+1));
                    nextRoomState.next = counts.get(result[i]+1);
                } else {
                    counts.put(result[i], nextRoomState.increase());
                }
            } else {
                if(counts.containsKey(room_idx-1) && counts.containsKey(room_idx+1)){
                    counts.put(room_idx, counts.get(room_idx+1));
                    counts.get(room_idx-1).next = counts.get(room_idx+1);
                } else if(counts.containsKey(room_idx-1)){
                    counts.put(room_idx, counts.get(room_idx-1).increase());
                } else if (counts.containsKey(room_idx+1)){
                    counts.put(room_idx, counts.get(room_idx+1));
                } else {
                    NextRoomState nextRoomState = new NextRoomState(room_idx + 1);
                    counts.put(room_idx, nextRoomState);
                }
                result[i] = room_idx;
            }
        }
        return result;
    }
}
```