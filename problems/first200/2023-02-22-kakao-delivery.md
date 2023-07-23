---
title: 카카오 코딩테스트, 택배 배달과 수거하기

date: 2023-2-22

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/150369?language=java)

택배로 배달과 재활용 가능한 박스의 수거를 한다. 선형적으로 배치된 n개의 집들이 있고, 집 간의 거리는 1이다. 택배 물류창고는 0번 째 집 왼쪽에 있고, 택배 물류창고와 0번 째 집까지의 거리도 1이다. 각 집에 배달할 양을 나타내는 배열 deliveries와 각 집에서 수거해야할 양을 나타내는 배열 pickups가 주어진다. 또한 택배 트럭의 용량인 1 이상의 정수가 cap으로 주어진다. 배달과 수거를 모두 완료하고 물류창고로 돌아오기까지 필요한 최소의 거리를 반환하라.

- 1 ≤ cap ≤ 50
- 1 ≤ n ≤ 100,000
- 0 ≤ deliveries의 원소 ≤ 50
- 0 ≤ pickups의 원소 ≤ 50

# 문제접근

1. 먼 곳 까지의 왕복을 최소화 하는게 효율적이기 때문에 가장 멀리 떨어져 있는 집부터 배달과 수거를 완료하는 것이 좋다. 가장 멀리 떨어진 곳부터 배달과 수거를 하는 것을 원칙으로 하되 만약 남는 용량이 있으면 그 앞 선 집에 배달 및 수거를 한다.

2. 배달과 수거를 하는 것은 독립적이다. 배달을 시작할 때는 최대 용량으로 배달을 시작하고, 수거를 시작할 때는 0에서 수거를 시작한다. 물류 창고에서 멀어 질 때는 배달만 하고, 물류 창고로 돌아올 때는 수거만 한다는 의미이다.

3. 배달과 수거는 독립적이지만, 이동하는 거리는 이 두 가지 모두에 영향을 받는다. 가장 먼 집에 배달만 있거나, 수거만 있거나 하여도 일단은 가장 먼 집까지 이동은 해야한다.

## 구현 방식

1. farHouse 변수로 배달/수거가 있는 가장 먼 집을 관리해준다. farHouse가 -1이면 배달이 완료되었음을 의미한다.

2. farHouse -1이 아니면 그곳까지 왕복하는 것이 확정이므로 dist에 그만큼을 더해준다.

3. farHouse까지 가면서 배달하는 만큼을 deliveries에서 빼준다. farHouse에서 물류창고로 오면서 수거하는 만큼을 pickups에서 빼준다.

4. 업데이트 된 deliveries와 pickups를 기준으로 가장 배달/수거가 있는 가장 먼 집을 갱신해준다.

# 코드

## 95% 정답, 5% 시간초과 : O(왕복횟수 \* 집 개수(n))

```java
class Solution {
    public long solution(int cap, int n, int[] deliveries, int[] pickups) {
        int farHouse = -1;
        for(int i = deliveries.length - 1; i >= 0; i -= 1){
            if(deliveries[i] != 0 || pickups[i] != 0){
                farHouse = i;
                break;
            }
        }
        long dist = 0;
        while(farHouse != -1){
            dist += (farHouse + 1) * 2;
            int deliveryCap = cap;
            int i = farHouse;
            while(deliveryCap > 0 && i >= 0){
                int deliveryAmount = deliveries[i] > deliveryCap ? deliveryCap : deliveries[i];
                deliveries[i] -= deliveryAmount;
                deliveryCap -= deliveryAmount;
                i -= 1;
            }
            int pickupCap = cap;
            int j = farHouse;
            while(pickupCap > 0 && j >= 0){
                int pickupAmount = pickups[j] > pickupCap ? pickupCap : pickups[j];
                pickups[j] -= pickupAmount;
                pickupCap -= pickupAmount;
                j -= 1;
            }
            for(int k = farHouse; k >= 0; k -= 1){
                if(deliveries[k] != 0 || pickups[k] != 0){
                    farHouse = k;
                    break;
                }
                if(k == 0){
                    farHouse = -1;
                }
            }
        }
        return dist;
    }
}
```
