---
title: 자바, 문자열 비교
date: 2023-8-6
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

이는 리터럴로 문자열을 생성하는 경우 같은 문자열 데이터를 가지는 경우 같은 객체를 가르키기 때문이다. 즉 name1과 name2는 따로 선언되었지만 같은 객체를 가르키고 있다. 메모리 효율성을 위해서 이렇게 설계되었다. 리터럴로 "jihun"을 생성하면 이 객체가 String Constant Pool에 저장되어서 재사용된다. 이렇게 해도 문제가 되지 않는 이유는 문자열이 immutable한 특성을 가지기 때문이다. 만약 mutable하다면 name1을 참조하면서 값을 바꾸면 name2도 바뀌는 예상치 못하는 동작이 일어날 것이다.

# 래퍼 객체 간의 비교

문자열 간의 비교뿐만 아니라 래퍼 객체 간 비교에서도 equals 메쏘드를 사용해야 비교를 성공적으로 수행한다.

```java
import java.util.*;

public class CompareWrapper {
     public static void main(String []args){
        List<Integer> list = new ArrayList<>();
        list.add((int) Math.pow(2, 25));
        list.add((int) Math.pow(2, 25));
        System.out.println(list.get(0) == list.get(1)); // false
        System.out.println(list.get(0).equals(list.get(1))); // true
     }
}
```

그런데 문자열과 다르게 래퍼 객체에 관해서는 이 점을 종종 간과하게 되는데, 그렇게 해도 문제가 되지 않는 경우가 있기 떄문이다. Integer형 작은 수의 경우는 == 로도 올바른 비교 결과를 반환한다. String Constant Pool과 같은 방식으로 많이 쓰이는 정수형 래퍼 객체의 경우 재사용 하기 때문이다.

```java
import java.util.*;

public class CompareWrapper {
     public static void main(String []args){
        List<Integer> list = new ArrayList<>();
        list.add((int) Math.pow(2, 3));
        list.add((int) Math.pow(2, 3));
        System.out.println(list.get(0) == list.get(1)); // true
        System.out.println(list.get(0).equals(list.get(1))); // true
     }
}
```

또한 원시 타입 int와 비교를 할 때도 == 으로 올바른 비교가 된다. 원시타입과 비교할 때는 컴파일러가 암묵적으로 unboxing을 해줘서 문제가 되지 않는 것으로 보인다.

```java
import java.util.*;

public class CompareWrapper {
     public static void main(String []args){
        List<Integer> list = new ArrayList<>();
        list.add((int) Math.pow(2, 25));
        System.out.println(list.get(0) == (int) Math.pow(2, 25));
     }
}
```
