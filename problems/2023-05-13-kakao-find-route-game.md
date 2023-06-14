---
title: 카카오 코딩테스트, 길 찾기 게임

date: 2023-5-13

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/42892)

이진트리의 각 노드가 x,y의 좌표로 주어진다. y는 노드의 레벨, x는 x축 상 노드의 위치를 나타낸다. 모든 노드의 x,y 좌표가 유효한 이진트리로 주어질 때, 이진트리를 전위순회한 결과와 후위순회한 결과를 반환하라.

- x축 상 위치는 모두 다르다. (유효한 이진트리 위치라면 반드시 그렇다.)
- 노드의 개수는 1이상 10,000이하이다.

# 문제접근

1. x,y 좌표 데이터로 트리틀 완전히 만들고, 전위순회, 후위순회 한 번씩 진행하면 된다.

2. 트리를 만들기 위해서 재귀함수를 활용했다. x 좌표를 기준으로 정렬을 먼저 한다. y 값이 가장 큰 것이 루트노드가 된다. x를 기준으로 정렬을 했기 때문에 루트노트 왼편으로 왼쪽 서브트리, 오른편으로 오픈쪽 서브트리가 된다. 서브트리에 같은 과정을 반복하면 완전히 연결된 트리를 얻을 수 있다.

# 구현
1. 자바에서 함수가 공통된 객체를 참조하기 위해서는(순회할 때 재귀함수가 하나의 List를 참조하는 것 같이) 객체를 계속 인자로 넘기거나, 클래스 변수로 선언해야한다. 

- 자바스크립트나 파이썬의 경우 렉시컬 환경, 상위 스코프 개념을 통해서 참조를 할 수 있지만 자바에는 그러한 것이 없다.


# 코드

```java
import java.util.Arrays;
import java.util.List;
import java.util.ArrayList;

class Node {
    int index;
    int x;
    int y;
    Node left;
    Node right;
    public Node(int x, int y, int index){
        this.index = index;
        this.x = x;
        this.y = y;
    }
}

class Solution {
    public int[][] solution(int[][] nodeinfo) {
        Node[] nodes = new Node[nodeinfo.length];
        for(int i = 0; i < nodeinfo.length; i += 1){
            nodes[i] = new Node(nodeinfo[i][0], nodeinfo[i][1], i+1);
        }
        Arrays.sort(nodes, (a, b) -> a.x - b.x);
        Node root = constructTree(nodes, 0, nodeinfo.length-1);
        List<Integer> preorder = new ArrayList<>(nodeinfo.length);
        preorderTraverse(root, preorder);
        List<Integer> postorder = new ArrayList<>(nodeinfo.length);
        postorderTraverse(root, postorder);
        return new int[][]{ preorder.stream().mapToInt(i->i).toArray(), postorder.stream().mapToInt(i->i).toArray() };
    }
    
    public static Node constructTree(Node[] nodes, int start, int end){
        if(start > end){
            return null;
        }
        int maxLevelArg = findMaxLevelArg(nodes, start, end);
        Node root = nodes[maxLevelArg];
        root.left = constructTree(nodes, start, maxLevelArg-1);
        root.right = constructTree(nodes, maxLevelArg+1, end);
        return root;
    }
    
    public static int findMaxLevelArg(Node[] nodes, int start, int end){
        int maxLevel = -1;
        int maxArg = -1;
        for(int i = start; i <= end; i += 1){
            if(maxLevel < nodes[i].y){
                maxLevel = nodes[i].y;
                maxArg = i;
            }
        }
        return maxArg;
    }

    public static void preorderTraverse(Node root, List<Integer> res){
        res.add(root.index);
        if(root.left != null){
            preorderTraverse(root.left, res);
        }
        if(root.right != null){
            preorderTraverse(root.right, res);
        } 
    }
    
    public static void postorderTraverse(Node root, List<Integer> res){
        if(root.left != null){
            postorderTraverse(root.left, res);
        }
        if(root.right != null){
            postorderTraverse(root.right, res);
        }
        res.add(root.index);
    }
}
```
