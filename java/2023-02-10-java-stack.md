---
title: 자바, 스택

date: 2023-2-10

categories:
  - java
---

# 스택

스택은 LIFO로 객체를 저장하고 조회하는 객체 집합이다. 스택 ADT는 push, pop, top, size 같은 연산을 지원해야 한다.

- 최근에 추가된 데이터를 먼저 조회하는데 유리한 데이터 구조이다.
- 유효한 괄호 판단 같이 나중에 추가된 데이터에서 부터 비교를 해야할 때 쓰일 수 있다.
- 그래프를 반복문을 통해 dfs로 조회할 때 많이 쓰이는 데이터 구조이다.

# java 구현체

```java
Stack<String> stack = new Stack<String>();
// stack에 데이터 추가(push)
stack.push("10");
stack.push("1");
stack.push("15");
// stack에서 데이터 꺼내기(pop)
System.out.println(stack.pop()); // 15
// stack 맨 위에 있는 값 조회(peek)
System.out.println(stack.peek()); // 1
// stack 크기 조회(size)
System.out.println(stack.size()); // 2
```
