---
title: 카카오 코딩테스트, 추석 트래픽

date: 2023-4-19

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/17676)

추석 하루에 대해서 1초 간 최대 처리량을 알아보려고 한다. 요청이 끝나는 시간과, 걸린 시간이 담긴 길이가 n인 배열이 주어질 때, 1초간 최대 처리량을 반환하라.

- n은 1 이상 2,000 이하
- 시간은 밀리세컨드 단위로 계산한다.
- 시작과 끝을 포함해서 1초를 샌다. (5001ms ~ 6000ms 가 1초이다.)

# 문제접근

1. 각 응답의 시작점과 끝점부터 1초간에 대해서 모든 응답과 비교를 해주면서, 1초 간 최대 처리량을 구해줄 수 있다. 한 응답의 시작과 끝점에 대해서 모든 응답과 비교를 하기 때문에 O(n^2)이다. 
        
        point1. 시작점과 끝점에서만 1초 간 최대 처리량의 새로운 값이 나올 수 있다. 

        point2. 시작점 및 끝점에서 시작하여 1초 간 겹치는 응답을 구해주면된다. 시작점에서 앞으로 1초간으로 겹치는 응답이 있을 수도 있다. 하지만 그런 경우는 앞 선 응답을 기준으로 하여 뒤로 1초 간을 하여 구할 때 이미 고려되었다.
  
2. 해설을 보면 O(nlogn) 풀이도 있다고 하니까 추후에 한 번 고민해봐도 좋을 것 같다. [해설](https://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1/)

# 코드

```java
import java.util.List;
import java.util.ArrayList;

class Time {
    public int milliSecond;
    public Time(String time){
        String[] timeStrArr = time.split(":");
        int hour = Integer.parseInt(timeStrArr[0]);
        int minute = Integer.parseInt(timeStrArr[1]);
        int milliSecond = Math.round(Float.parseFloat(timeStrArr[2]) * 1000);
        this.milliSecond = hour * 3600 * 1000 + minute * 60 * 1000 + milliSecond;
    }

    public Time(int milliSecond){
        this.milliSecond = milliSecond;
    }

    public Time minusMilliSecond(int milliSecond){
        int ms = this.milliSecond - milliSecond;
        return new Time(ms < 0 ? 0 : ms);
    }
}

class Response {
    public Time startTime;
    public Time endTime;
    public Response(Time startTime, Time endTime){
        this.startTime = startTime;
        this.endTime = endTime;
    }
    public boolean hasOverlapTimeOn(int startMilliSecond, int endMilliSecond){
        if(this.startTime.milliSecond > endMilliSecond || this.endTime.milliSecond < startMilliSecond){
            return false;
        }
        return true;
    }
}

class Solution {
    public int solution(String[] lines) {
        List<Response> responses = new ArrayList<>();
        for(int i = 0; i < lines.length; i += 1){
            String[] log = lines[i].split(" ");
            String time = log[1];
            int duration = Math.round(Float.parseFloat(log[2].substring(0, log[2].length()-1)) * 1000);
            Time endTime = new Time(time);
            Time startTime = endTime.minusMilliSecond(duration - 1);
            responses.add(new Response(startTime, endTime));
        }
        int max_count = 0;
        for(Response res: responses){
            Time startTime = res.startTime;
            Time endTime = res.endTime;
            int startTimeAroundCount = 0;
            int endTimeAroundCount = 0;
            for(Response res2: responses){
                if(res2.hasOverlapTimeOn(startTime.milliSecond, startTime.milliSecond + 999)){
                    startTimeAroundCount += 1;
                }
                if(res2.hasOverlapTimeOn(endTime.milliSecond, endTime.milliSecond + 999)){
                    endTimeAroundCount += 1;
                }
            }
            max_count = Math.max(max_count, startTimeAroundCount);
            max_count = Math.max(max_count, endTimeAroundCount);
        }
        return max_count;
    }
}
```
