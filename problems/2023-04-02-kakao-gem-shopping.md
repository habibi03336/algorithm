---
title: 카카오 코딩테스트, 보석 쇼핑

date: 2023-4-2

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/67258)

크기가 n인 보석의 배열이 주어진다. 연속적으로 놓인 보석을 a부터 b까지 선택한다. 이 때 각 종류의 보석을 적어도 하나씩은 선택하면서도, 총 선택하는 보석의 개수를 최소화(b-a를 최소화)하는 a,b를 정수 배열로 반환하라.

- n은 1이상 100,000이하
- 보석의 종류는 길이가 1이상 10이하인 문자열로 주어짐


# 문제접근
1. `애벌레 방식`을 적용하여 풀었다. 매번 새롭게 탐색을 해주는 것이 아니다. 이미 앞서서 탐색된 내용에서 중간은 남긴다. 그리고 앞쪽에서 빠지는 보석과 뒤쪽에서 추가되는 보석만 고려해주면된다. 마치 애벌레가 꿈틀거리며 앞으로 가는 것 같은 모습이다. 각 요소를 상수번 접근하기 때문에 O(n) 풀이이다. 

        t번째에서 q까지에서 모든 종류의 보석을 선택한다면, 
        t+1에서 q까지도 모든 종류의 보석이 선택되거나 아니면 거의 대부분의 종류의 보석이 선택되었을 것이다. 


# 코드

## 조금 정리한 코드

- answer에 기본 값을 넣어줘서 if 조건문을 간략하게 함
- 이전 보석을 제외해주는 것을 for문 마지막 부분으로 빼서 start > 0 조건문을 제거
- 변수명 조금 수정

```java
import java.util.*;

class Solution {
    public int[] solution(String[] gems) {
        Map<String, Integer> counts = new HashMap<>();
        for(int i = 0; i < gems.length; i += 1){
            counts.put(gems[i], 0);
        }
        int kindNum = counts.keySet().size();
        int uniqueCount = 0;
        int[] answer = new int[]{1, gems.length};
        int end = 0;
        for(int start = 0; start < gems.length; start += 1){
            while(uniqueCount != kindNum && end < gems.length){
                if(counts.get(gems[end]) == 0){
                    uniqueCount += 1;
                }
                counts.put(gems[end], counts.get(gems[end]) + 1);
                end += 1;
            }
            
            if(
                uniqueCount == kindNum && 
                answer[1] - answer[0] + 1 > end - start 
            ){
                answer = new int[]{start + 1, end};
            }
            

            if(counts.get(gems[start]) == 1){
                uniqueCount -= 1;
            }
            counts.put(gems[start], counts.get(gems[start]) - 1);
        }
        return answer;
    }
}
```

## 정리가 안 된 코드

```java
import java.util.*;

class Solution {
    public int[] solution(String[] gems) {
        Map<String, Integer> counts = new HashMap<>();
        for(int i = 0; i < gems.length; i += 1){
            counts.put(gems[i], 0);
        }
        int gemKindNum = counts.keySet().size();
        int uniqueCount = 0;
        int[] answer = null;
        int end = 0;
        for(int i = 0; i < gems.length; i += 1){
            int start = i;
            if(start > 0){
                if(counts.get(gems[start-1]) == 1){
                    uniqueCount -= 1;
                }
                counts.put(gems[start-1], counts.get(gems[start-1]) - 1);
            }
            while(uniqueCount != gemKindNum && end < gems.length){
                if(counts.get(gems[end]) == 0){
                    uniqueCount += 1;
                }
                counts.put(gems[end], counts.get(gems[end]) + 1);
                end += 1;
            }
            if(
                uniqueCount == gemKindNum && (
                    answer == null ||
                    answer[1] - answer[0] + 1 > end - start
                )
            ){
                answer = new int[]{start + 1, end};
            }
        }
        return answer;
    }
}
```