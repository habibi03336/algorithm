---
title: 백준, 20168. 골목 대장 호석

date: 2023-5-24

categories:
  - 문제풀이

tags:
---

# [문제](https://www.acmicpc.net/problem/20168)

<div style="text-align: center;">
    <img src="/assets/img/bj_20168.jpeg" alt="bj_20168"/>
</div>


# 코드

```java
import java.io.IOException;
import java.util.*;

public class bj_20168 {
    public static void main(String[] args) throws IOException {
        //input
        Scanner scanner = new Scanner(System.in);
        int N = scanner.nextInt();
        int M = scanner.nextInt();
        int start= scanner.nextInt();
        int destination = scanner.nextInt();
        int money = scanner.nextInt();
        Map<Integer, List<int[]>> graph = new HashMap<>();
        for(int i = 0; i < M; i += 1){
            int a = scanner.nextInt();
            int b = scanner.nextInt();
            int cost = scanner.nextInt();
            if(graph.containsKey(a)){
                graph.get(a).add(new int[]{b, cost});
            } else {
                List neighbors = new ArrayList<>();
                neighbors.add(new int[]{b, cost});
                graph.put(a, neighbors);
            }
            if(graph.containsKey(b)){
                graph.get(b).add(new int[]{a, cost});
            } else {
                List neighbors = new ArrayList<>();
                neighbors.add(new int[]{a, cost});
                graph.put(b, neighbors);
            }
        }
        //output
        System.out.println(Integer.toString(solution(graph, start, destination, money, N)));
    }

    public static boolean[] visited;
    public static List<Integer> costList;
    public static int minMaxCost;

    public static int solution(Map<Integer, List<int[]>> graph, int start, int destination, int money, int nodeNum){
        visited = new boolean[nodeNum+1];
        minMaxCost = Integer.MAX_VALUE;
        costList = new ArrayList<>();
        dfs(graph, start, destination, 0, money);
        return minMaxCost == Integer.MAX_VALUE ? -1 : minMaxCost;
    }

    public static void dfs(Map<Integer, List<int[]>> graph, int startNode, int endNode, int cost, int moneyLimit){
        if(startNode == endNode){
            minMaxCost = Math.min(minMaxCost, Collections.max(costList));
            return;
        }
        for(int[] nextNode : graph.get(startNode)){
            if(visited[nextNode[0]]){
                continue;
            }
            if(minMaxCost < nextNode[1]){
                continue;
            }
            if(cost + nextNode[1] > moneyLimit){
                continue;
            }
            visited[nextNode[0]] = true;
            costList.add(nextNode[1]);
            dfs(graph, nextNode[0], endNode, cost + nextNode[1], moneyLimit);
            costList.remove(costList.size()-1);
            visited[nextNode[0]] = false;
        }
    }
}
```
