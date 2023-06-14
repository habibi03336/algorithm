---
title: 카카오 코딩테스트, 외벽 점검

date: 2023-5-15

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/60062)

원 모양의 외벽이 있다. 외벽 군데군데 취약점이 있다. 취약점이 더 심해졌는지 확인하려고 한다. 친구들에게 확인을 부탁하려고 하는데 친구들이 움직일 수 있는 거리는 정해져있고 각각 다르다. 
원 모양 외벽의 둘레, 취약점의 위치, 친구들이 움직일 수 있는 거리가 주어질 때, 부탁해야하는 친구의 최소는?

- 외벽의 둘레는 1이상 200이하이다.
- 취약점의 개수는 1이상 15개 이하이다.
- 친구들은 1명 이상 8명 이하이고 움질일 수 있는 거리는 모두 다르다.
- 점검을 시작하는 위치는 자유롭게 선택할 수 있다.

# 문제접근

1. 브루트포스로 접근했다. 점검을 시작하는 위치의 경우의 수는 15C8개이고, 각 경우에 친구를 배치하는 경우의 수는 8!이다. 확인해보니 15C8이 6,435이고, 8!이 40,320이다. 둘을 곱하면 대략 2.6억 정도 나온다. 억 단위로 넘어가면 보통 초 단위로 작동을 한다. 그래서 그렇게 효율적인 알고리즘은 아니다. 하지만 다른 접근이 잘 안그려져서 브루트포스를 활용했다.

2. 문제에 효율성 조건이 없는 것으로 보인다. 테스트케이스에서 최대 8.3초까지 걸렸다.

# 구현

1. dfs를 통해서 브루트포스를 구현했다. 취약점을 한 번씩 시작점 삼아, 갈 수 있는 거리만큼까지 취약점 확인해주고, 다음 친구를 조회하는 것을 재귀 호출한다.  

# 코드

```java
class Solution {
    private int res = 100;
    public int solution(int n, int[] weak, int[] dist) {    
        int[] slots = new int[n];
        for(int i = 0; i < n; i += 1){
            if(isWeakPoint(weak, i)){
                slots[i] = 1;
            }
        }
        dfs(slots, dist, dist.length - 1);
        return this.res == 100 ? -1 : this.res;
    }   
    
    public void dfs(int[] slots, int[] dist, int distIndex){
        if(allSlotChecked(slots)){
            this.res = Math.min(this.res, dist.length - distIndex - 1);
            return;
        }
        if(distIndex < 0){
            return;
        }
        for(int i = 0; i < slots.length; i += 1){
            if(slots[i] != 1){
                continue;
            }
            int start = i;
            int[] temp = new int[dist[distIndex]+1];
            for(int j = 0; j < dist[distIndex] + 1; j += 1){
                int walk = (start + j) % slots.length;
                temp[j] = slots[walk];
                slots[walk] = 0;
            }
            dfs(slots, dist, distIndex - 1);
            for(int j = 0; j < dist[distIndex] + 1; j += 1){
                int walk = (start + j) % slots.length;
                slots[walk] = temp[j];
            }
        }
    }
    
    public static boolean allSlotChecked(int[] slots){
        for(int i = 0; i < slots.length; i += 1){
            if(slots[i] == 1){
                return false;
            } 
        }
        return true;
    }
    
    public static boolean isWeakPoint(int[] weak, int index){
        for(int i = 0; i < weak.length; i+= 1){
            if(weak[i] == index){
                return true;
            }
        }
        return false;
    }
}
```
