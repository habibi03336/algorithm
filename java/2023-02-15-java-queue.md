---
title: 자바, 큐

date: 2023-2-15

categories:
  - java
---

# 큐

큐는 FIFO로 객체를 저장하고 조회하는 객체 집합이다. 큐 ADT는 enqueue, dequeue 두 가지의 주요 업데이트 메쏘드를 제공하고, first, size와 같은 추가적인 메쏘드를 제공한다. 큐 자료구조는 배열이나 linked list로 구현 할 수 있다.

- 가장 예전에 추가된 객체가 가장 먼저 나온다. (가장 먼저 추가된 객체가 먼저 나온다)
- while, for문과 함께 bfs로 그래프를 탐색할 때 쓰일 수 있다.
- 시간 지역성을 이용하는 캐싱에서 캐시 교체 알고리즘으로 많이 쓰인다.

# LinkedList 구현체를 Queue로 사용하기

자바 문서에 따르면 Queue는 Collection, Iterable 인터페이스를 상속받고, 추가로 add, element, offer, peek, poll, remove 메쏘드를 구현해야한다. LinkedList는 Collection이고 Iterable이자 Queue가 요구하는 모든 메쏘드를 구현하고 있다. 또한 작동도 FIFO로 한다.

[📑 Queue를 정의하는 오라클 자바 문서](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)

```java
Queue<String> que = new LinkedList<>();
// que에 추가
que.add("안녕하세요");
que.add("제 이름은 하지훈입니다.");
que.add("만나서 반가워요.");
// que에서 가장 처음 것 조회
System.out.println(que.peek()); // 안녕하세요
// que에서 가장 처음 것을 추출
System.out.println(que.poll()); // 안녕하세요
System.out.println(que.poll()); // 제 이름은 하지훈입니다.
```
