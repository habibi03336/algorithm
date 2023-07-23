---
title: 연습문제, 이진 트리, 경로의 합이 K가 되는 경우 수

date: 2023-5-24

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제

<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/habibi03336/algorithm/master/assets/img/exercise_4_12.jpeg" alt="exercise_4_12"/>
</div>

# 테스트 케이스

트리 배열 표현시 크기, 목표 값 k, 노드 값들 (null일 경우 -1)

(사실 문제조건상 노드 값이 음수일수도 있어서 -1로 하면 안 됨)

    1. 15 6
       10 5 -3 3 1 -1 11 3 -2 -1 2 -1 -1 -1 -1
       expected: 3

# 코드

```java
import java.io.IOException;
import java.util.*;

class Node {
    int val;
    public Node left;
    public Node right;
    public Node(int num){
        this.val = num;
    }
}

public class exercise_4_12 {

    public static void main(String[] args) throws Exception {
        //input
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int targetSum = scanner.nextInt();
        int[] vals = new int[n];
        for(int i = 0; i < n; i += 1){
            vals[i] = scanner.nextInt();
        }
        Node root = constructTree(vals);

        //output
        System.out.println(Integer.toString(solution(root, targetSum)));
    }

    static private Node constructTree(int[] vals) throws Exception {
        if(vals.length == 0) throw new Exception("at least root value should be given");
        Node[] nodes = new Node[vals.length];
        for(int i = 0; i < vals.length; i += 1){
            if(vals[i] == -1){
                continue;
            }
            nodes[i] = new Node(vals[i]);
        }
        Queue<Integer> que = new LinkedList<>();
        que.add(0);
        while(que.size() != 0){
            int i = que.poll();
            if(i*2+1 < vals.length && nodes[i*2+1] != null) {
                nodes[i].left = nodes[i*2+1];
                que.add(i*2+1);
            }
            if(i*2+2 < vals.length && nodes[i*2+2] != null){
                nodes[i].right = nodes[i*2+2];
                que.add(i*2+2);
            }
        }
        return nodes[0];
    }

    public static int count = 0;
    public static int solution(Node root, int targetSum){
        dfs(root, targetSum);
        return count;
    }
    public static int[] dfs(Node root, int targetSum){
        if(root == null){
            return new int[]{};
        }
        int[] leftStates = dfs(root.left, targetSum);
        int[] rightStates = dfs(root.right, targetSum);
        int[] states = new int[leftStates.length + rightStates.length + 1];
        states[0] = root.val;
        for(int i = 0; i < leftStates.length; i += 1){
            states[1+i] = root.val + leftStates[i];
        }
        for(int i = 0; i < rightStates.length; i+= 1){
            states[1+ leftStates.length + i] = root.val + rightStates[i];
        }
        for(int i = 0; i < states.length; i += 1){
            if(states[i] == targetSum){
                count += 1;
            }
        }
        return states;
    }
}

```
