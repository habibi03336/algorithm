---
title: 카카오 코딩테스트, 징검다리

date: 2023-3-31

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/64062)

여러 명의 사람들이 징검다리를 건넌다. 징검다리는 일정한 간격으로 배치되어 있다. 징검다리는 바로 앞에 있는 것부터 밟는다. 하지만 징검다리는 밟을 수 있는 횟수가 제한되어있다. 가능한 횟수를 다 밟으면 그 징검다리는 없는 것이 된다. 각 징검다리를 밟은 수 있는 횟수가 길이가 n인 배열로 주어진다. 또한 한 번에 뛰어넘을 수 있는 징검다리의 수를 나타내는 1 이상 정수 k가 주어진다. 이 때 징검다리를 건널 수 있는 인원의 최대 수를 구하여라.

- 배열의 길이 n은 1이상 200,000이하
- 배열의 요소의 크기는 1이상 200,000,000이하
- k는 stones의 길이 이하 자연수

# 문제접근

1. 가만 생각해보면 t번 째 징검다리에 도달할 수 있는 사람의 수는, t - k, t - k + 1, ... t -1 까지 올 수 있는 사람의 수 중 최대값이다. 

        t - k 번 징검다리를 밟은 사람과, t - 1번 징검다리를 밟은 사람이 다른 사람이고, 그래서 t번 징검다리에 (t-k번 을 밟은 사람의 수) +  (t-1번을 밟은 사람의 수)가 도달할 수 있다고 생각할 수 있다. 하지만 이는 착각이다. 왜냐하면 가능한 가까운 징검다리를 모두 밟아야 한다는 조건이 있기 때문이다. 

2. 따라서 DP 방식으로 t번 째 징검다리에 도달할 수 있는 사람의 수로 앞 선 각 k개의 징검다리에 도달할 수 있는 사람 수 중 최대값을 취해주면 된다. 이 때 매번 k개의 징검다리를 모두 조회한다면 O(n*k)의 시간복잡도를 가지게된다. 문제 조건상 시간초과가 날 확률이 높다.

3. 따라서 최적화가 필요하다. 이 문제는 큐에서 pop과 add가 반복적으로 일어날 때 최댓값을 효율적으로 접근하는 방법을 고안하는 것이라고 할 수 있다. 나는 우선순위큐를 활용하는 방식으로 최적화를 해줬다. 큐 자료구조를 직접적으로 사용하지는 않았고, 큐에 인덱스와 값을 가지는 노드를 넣어주고, 값 기준으로 우선순위 큐에서 빼주면서 만약 인덱스가 큐에 들어있을 수 없는 것이면 무시했다. 우선순위 큐에 최대 2k만큼의 노드가 들어갈 수 있다. 또한 우선순위 큐에 모든 노드가 한 번씩 들어갔다 나온다고 할 수 있다. 따라서 시간복잡도는 O(n*logK)라고 할 수 있다.

# 코드
```java
import java.util.*;

class Node implements Comparable<Node> {
    public int index;
    public int val;
    
    public Node(int index, int val){
        this.index = index;
        this.val = val;
    }
    
    public int compareTo(Node o) {
         if(val < o.val){
             return 1;
         } else if(val > o.val){
             return -1;
         } else{
             return 0;
         }
    }
}

class Solution {
    public int solution(int[] stones, int k) {
        PriorityQueue<Node> pq = new PriorityQueue<>();
        for(int i = 0; i < k; i += 1){
            pq.add(new Node(i, stones[i]));
        }
        for(int i = k; i < stones.length; i += 1){
            while(pq.peek().index < i - k){
                pq.poll();
            }
            int priorMax = pq.peek().val;
            int curMax = priorMax > stones[i] ? stones[i] : priorMax;
            pq.add(new Node(i, curMax));
        }
         while(pq.peek().index < stones.length - k){
            pq.poll();
        }
        return pq.peek().val;
    }
}
```