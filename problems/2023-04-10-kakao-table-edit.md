---
title: 카카오 코딩테스트, 표 편집

date: 2023-4-10

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/81303)

여러 개의 행으로 이루어진 표를 편집하는 프로그램을 만든다. 프로그램이 가진 명령어는 다음과 같다
1. U {num} (ex. U 5): 위 쪽 {num}번 째 행을 선택한다.
2. D {num} (ex. D 5): 아래 쪽 {num}번 째 행을 선택한다.
3. C: 현재 행을 삭제한다. 선택은 다음 행으로 넘어간다. 만약 다음 행이 없으면 그 전 행을 선택한다.
4. Z: 가장 최근에 삭제한 행을 복구한다. 선택된 행은 바뀌지 않는다.

행의 개수, 시작하는 행, 명령어 배열이 주어진다. 이 때 처음과 비교하여 삭제된 행은 X, 삭제되지 않은 행은 O로 표시하여 문자열로 반환하라.

- 행의 범위를 벗어나는 선택 이동 명령은 주어지지 않는다.
- 삭제된 것이 없는데 Z 명령어를 사용하는 경우는 없다.
- 행의 개수 n은 5 이상 1,000,000 이하이다.
- 이동 명령의 범위 d는 1 이상 300,000 이하이다.
- 명령의 횟수는 1 이상 300,000 이하이다.


# 문제접근

1. 배열을 활용하여 랜덤엑세스로 선택 이동 명령시 O(1)으로 하되, 삭제나 복구시 O(n)으로 작동하게 할 수 있다.

2. Doubly Linked List를 활용하여 선택 이동 명령시 O(d)으로 하되, 삭제나 복구시 O(1)으로 작동하게 할 수 있다.

3. 문제 조건 상 O(d)나 O(n)이 있으면 시간초과가 날 수도 있을 것 같다고 생각해서 좀 더 고민을 해봤다. 하지만 모든 작업을 선형이나 로그 시간으로 할 수 있는 방법이 없을 것 같았다. 따라서 좀 더 성능이 괜찮은 Doubly Linked List로 접근하였다. 걱정과 달리 문제상 시간초과는 없었다.

# 코드

```java
import java.util.Stack;

class Node {
    public Node prior = null;
    public Node next = null;
    public int value;
    public Node(int value){
        this.value = value;
    }
}

class Solution {
    public String solution(int n, int k, String[] cmd) {
        Node head = new Node(-1);
        Node node = head;
        for(int i = 0; i < n; i += 1){
            Node nextNode = new Node(i);
            node.next = nextNode;
            nextNode.prior = node;
            node = nextNode;
        }
        Node tail = new Node(-1);
        node.next = tail;
        tail.prior= node;
        
        node = head;
        for(int i = 0; i < k + 1; i += 1){
            node = node.next;
        }
        
        Stack<Node> stack = new Stack<>();
        for(int i = 0; i < cmd.length; i += 1){
            String[] command = cmd[i].split(" ");
            String commandType = command[0];
            if(commandType.equals("U")){
                int jumpCount = Integer.parseInt(command[1]);
                for(int j = 0; j < jumpCount; j += 1){
                    node = node.prior;
                }
            } 
            if(commandType.equals("D")){
                int jumpCount = Integer.parseInt(command[1]);
                for(int j = 0; j < jumpCount; j += 1){
                    node = node.next;
                }
                
            }
            if(commandType.equals("C")){
                Node nextNode = node.next;
                Node priorNode = node.prior;
                nextNode.prior = priorNode;
                priorNode.next = nextNode;
                stack.add(node);
                if(nextNode.value != -1){
                    node = nextNode;
                } else{
                    node = priorNode;
                }
            }
            if(commandType.equals("Z")){
                Node restoredNode = stack.pop();
                Node priorNode = restoredNode.prior;
                Node nextNode = restoredNode.next;
                nextNode.prior = restoredNode; 
                priorNode.next = restoredNode;
            }
        }
        node = head.next;
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < n; i += 1){
            if(node.value == i){
                sb.append("O");
                node = node.next;
            } else {
                sb.append("X");
            }
        }
        return sb.toString();
    }
}
```
