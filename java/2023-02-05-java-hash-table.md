---
title: 자바, 해쉬 테이블
date: 2023-2-5
categories:
  - java
---

# 해쉬 테이블

해쉬 테이블은 key와 value가 쌍을 이루는 추상 자료형(ADT)이다. key 값으로 value를 접근할 수 있다. key를 알고 있으면 value에 접근하는 시간 복잡도는 O(1)이다. 배열과 해싱을 활용하여 구현한다. key를 해싱하는 과정에 약간의 오버헤드가 있다. 

또한 서로 다른 두 키를 해싱하였는데 같은 값이 나올 수 있다. 이를 '충돌(collision)'이라고 한다. 보통 선형 프로빙이나 체이닝 방식으로 충돌 문제를 해결하는데 자바의 HashMap 같은 경우 체이닝 방식을 활용한다. 또한 충돌이 너무 많이 일어나는 경우 Singly Linked List 방식의 체이닝을 Red Black Tree로 변환하여 관리한다.

# 구현체

## HashMap

```java
HashMap<String, Integer> hm = new HashMap<String, Integer>();
hm.put("jihun", 10);
hm.get("jihun"); // 10
hm.get("notExistKey"); // null
```

HashTable, ConcurrentHashMap과 같은 다른 구현체도 있다. [멀티쓰레딩 환경과 관려하여 사용할 해쉬테이블을 달리한다.](https://tecoble.techcourse.co.kr/post/2021-11-26-hashmap-hashtable-concurrenthashmap/)
