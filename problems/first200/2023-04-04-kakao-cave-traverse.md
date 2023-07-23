---
title: 카카오 코딩테스트, 동굴 탐험

date: 2023-4-3

categories:
  - 문제풀이

tags:
  - Exercise
  - 🧑🏻‍💻
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/67260)

n개의 방으로 이루어진 트리 구조의 동굴을 탐험한다. 루트부터 시작해 모든 방(노드)를 방문하려고 한다. 동굴 탐험을 규칙에 따라 하려고 하는데, 그 규칙이란 노드 A를 방문하기 전에 노드 B를 방문해야한다는 식이다. 규칙의 조건은 다음과 같다.

1. 특정 방에 대한 방문 조건은 1개이거나 없다.
    A -> B 조건과, C -> B 조건이 같이 있을 수 없다.
2. 서로 다른 방에 같은 방문 조건이 있을 수는 없다.
    A -> C 조건과, A -> D 조건이 같이 있을 수 없다.
3. 하나의 방에 도달하는데 방문조건이 있고, 그 방이 또 다른 방의 방문조건인 경우는 없다.
    A -> C -> D 인 조건은 없다.

규칙을 따라 동굴을 탐험할 수 있는지 true or false를 반환하라.

- n은 1이상 200,000 이하이다.

# 문제접근
1. 방문한 노드의 상태를 업데이트하고, 반복적으로 dfs를 호출해 풀 수 있다고 생각했다. 
        
        1. 루트부터 시작해 dfs를 한다. 방문할 수 있는 최대한의 노드를 방문한다. 조건 상 방문하지 못한 노드를 저장해둔다.
        2. 방문한 노드가 늘었으므로, 앞서서 방문하지 못한 노드를 시작점으로 다시 dfs한다.
        3. 방문한 노드가 늘면 dfs를 계속하고, 방문한 노드가 같으면 더 이상 업데이트가 없다는 의미이므로 반복을 멈춘다.
        4. 방문한 노드 개수가 전체 노드 개수면 true, 아니면 false를 반환한다.

# 코드

효율성에서 마지막 하나가 시간초과가 난다.

```java
import java.util.*;

class Node {
    public int val;
    public ArrayList<Node> children = new ArrayList<>();
    public int shouldVisited = -1; 
        
    public Node(int val){
        this.val = val;
    }
}

class Solution {
    private Set<Integer> visited = new HashSet<>();
    public boolean solution(int n, int[][] path, int[][] order) {
        Node[] nodes = new Node[n];
        for(int i = 0; i < n; i += 1){
            nodes[i] = new Node(i);
        }
        for(int[] p : path){
            nodes[p[0]].children.add(nodes[p[1]]);
            nodes[p[1]].children.add(nodes[p[0]]);
        }
        for(int[] o : order){
            nodes[o[1]].shouldVisited = o[0];
        }
        List<Node> startNodes = new ArrayList<>();
        startNodes.add(nodes[0]);
        while(true){
            int tmp = visited.size();
            startNodes = dfs(startNodes);
            if(tmp == visited.size()){
                break;
            }
        }
        return visited.size() == n;
    }
    
    private List<Node> dfs(List<Node> startNodes){
        List<Node> nextStartNodes = new ArrayList<>();
        dfs(startNodes, nextStartNodes);
        return nextStartNodes;
    }
    
    private void dfs(List<Node> startNodes, List<Node> nextStartNodes){
        for(Node node : startNodes){
            if(visited.contains(node.val)){
                continue;
            }
            if(node.shouldVisited != -1 && !visited.contains(node.shouldVisited)){
                nextStartNodes.add(node);
            } else {
                visited.add(node.val);
                dfs(node.children, nextStartNodes);
            }
        }
    }
}
```