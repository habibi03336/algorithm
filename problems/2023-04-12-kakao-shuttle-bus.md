---
title: 카카오 코딩테스트, 셔틀 버스

date: 2023-4-12

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/17678)

셔틀 버스가 오전 9시부터 t분 간격으로 n회 온다. 사람들이 줄을 서 있다가 먼저 온 순서대로 셔틀버스를 탄다. 셔틀이 도착한 시간까지 줄을 선 사람 중에 셔틀 용량인 m명까지 순서대로 탄다. 사람들이 줄을 서는 시간이 timetables로 주어질 때, 마지막 셔틀에 마지막 순번으로 타기위해 줄을 서야 하는 시간을 구하시오.

- 셔틀이 도착한 시간에 줄을 설 사람도 셔틀을 탈 수 있다.
- 동일한 시간에 도착한 사람들이 있다면 가장 마지막에 선다고 가정한다.

# 문제접근

1. 시뮬레이션 방식으로 문제를 풀려고 했다. 셔틀이 오는 시간 순서대로 큐를 만들고, 사람들이 오는 시간 순서대로 큐를 만든다. 가장 앞선 셔틀 부터 조회하면서 셔틀이 도착하는 시간 전에 도착한 사람을 순차적으로 셔틀 용량까지 큐에서 꺼내준다. 이를 반복하다가 마지막 셔틀인 경우 셔틀이 꽉 차면 마지막 사람보다 1분 일찍 줄을 선다. 마지막 셔틀인데 꽉 차지 않으면 셔틀이 도착하는 그 시간에 줄을 선다.

# 구현

1. 시간을 다룰 때는 조건상 가장 작은 단위의 시간으로 변환하여 다루는 것이 편하다. 시간 연산이 매우 간단해 진다. 

2. 별도의 구조체를 만들어서 데이터를 관리하는 것이, 코드 상도 보기 좋고 생각을 단순화 하는데에도 큰 도움이 된다.

# 코드

```java
import java.util.*;

class Time {
    private int minute;
    public Time(int minute){
        this.minute = minute;
    }
    public Time(String timeStr){
        String[] time = timeStr.split(":");
        minute = Integer.parseInt(time[0]) * 60 + Integer.parseInt(time[1]);
    }
    public int inMinute(){
        return this.minute;
    }
    
    public String toString(){
        int h = minute / 60;
        int m =  minute % 60;
        String hourString = h < 10 ? String.format("0%d", h) : String.format("%d", h);
        String minuteString = m < 10 ? String.format("0%d", m) : String.format("%d", m);
        return String.format("%s:%s", hourString, minuteString);
    }
}

class Solution {
    public String solution(int n, int t, int m, String[] timetable) {
        Queue<Time> shuttleQue = new LinkedList<>();
        int shuttleStartTime = 9 * 60;
        for(int i = 0; i < n; i += 1){
            Time shuttleTime = new Time(shuttleStartTime + t*i);
            shuttleQue.add(shuttleTime);
        }
        LinkedList<Time> crewQue = new LinkedList<>();
        for(int i = 0; i < timetable.length; i += 1){
            crewQue.add(new Time(timetable[i]));
        }
        Collections.sort(crewQue, (o1, o2) -> {
            return o1.inMinute() - o2.inMinute();
        });
        Time answer = null;
        while(shuttleQue.size() != 0){
            int curMinute = shuttleQue.peek().inMinute();
            int count = 0;
            Time lastCrew = null;
            while(crewQue.size() != 0 && crewQue.peek().inMinute() <= curMinute && count < m){
                lastCrew = crewQue.poll();
                count += 1;
            }
            if(shuttleQue.size() == 1 && count == m){
                answer = new Time(lastCrew.inMinute() - 1);
            } 
            if(shuttleQue.size() == 1 && count < m){
                answer = new Time(curMinute);
            }
            shuttleQue.poll();
        }
        
        return answer != null ? answer.toString() : null;
    }
}
```
