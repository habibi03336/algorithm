---
title: 자바, 문자열 비교
date: 2023-2-5
categories:
  - java
---

# 문자열 비교 방법

```java
String name1 = new String("jihun");
String name2 = new String("jihun");
System.out.println(name1.equals(name2)); // true
System.out.println(name1 == name2); // false
```

자바에서 객체의 비교(==)는 두 객체가 같은지를 반환한다. name1과 name2는 같은 문자열 데이터를 가지고 있지만 서로 다른 객체이다. 따라서 등호로 비교하면 false가 나온다. 문자열 데이터가 같은지 비교하고 싶다면 equals 메쏘드를 사용해야한다.

다만! 리터럴로 문자열을 생성하면 등호가 성립한다.

```java
String name1 = "jihun";
String name2 = "jihun";
System.out.println(name1 == name2); //true
```

이는 리터럴로 문자열을 생성하는 경우 같은 문자열 데이터를 가지는 경우 같은 객체를 가르키기 때문이다. 즉 name1과 name2는 따로 선언되었지만 같은 객체를 가르키고 있다. 효율성을 위해서 이렇게 설계되었다.

이렇게 해도 문제가 되지 않는 이유는 문자열이 immutable한 특성을 가지기 때문이다. 만약 mutable하다면 name1을 참조하면서 값을 바꾸면 name2도 바뀌는 예상치 못하는 동작이 일어날 것이다.
