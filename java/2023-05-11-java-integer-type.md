---
title: 자바, 범위 조건에 따른 정수형 그리고 BigIntger
date: 2023-5-11
categories:
  - java
---

# 자바에서 정수형 종류
1. int(Integer): 4Byte, (-2,147,483,648 ~ 2,147,483,647 | 약 +,- 20억) 

2. long(Long): 8Byte, (-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 | 약 +,- 90경)

3. BigInteger: 동적 배열 형태로 숫자의 범위 무제한

# BigInteger의 사용
웬만하면 long의 범위를 넘어가는 경우가 흔하지는 않겠지만, [경우의 수를 구한다거나 하는 경우에 long의 범위가 부족할 수 있다.](https://www.acmicpc.net/problem/2407) 이럴 때 BigIntger를 사용할 수 있다.

BigIntger는 아래와 같은 엄청나게 큰 수에 대한 덧셉을 비교적 매우 빠르게 계산한다. 배열 형태로 되어있다면 숫자가 커질수록 연산의 성능이 빠르게 떨어져야 할 것 같은데 어떻게 이런 성능이 가능할까?

```java
StringBuilder veryLongNum = new StringBuilder();
for(int i = 0; i < 100; i += 1) {
    veryLongNum.append(Long.toString(Long.MAX_VALUE));
}
BigInteger big1 = new BigInteger(veryLongNum.toString());
BigInteger big2 = new BigInteger(veryLongNum.toString());
System.out.println(big1.add(big2).toString());
```

---

# [BigInteger의 구현](http://www.docjar.com/html/api/java/math/BigInteger.java.html)
BigIntger 구현의 소스코드를 찾아볼 수 있었다. 더하기를 어떻게 하는지 이해하기 위해서는 값의 절대적 크기를 의미하는 `mag`가 어떤 데이터인지부터 파악할 필요가 있다. 문자열로부터 BigInteger를 생성하는 것을 확인하여 mag가 어떻게 절대적 크기를 나타내는지 파악해보자.

## 생성자
생성자를 통해 확인한 결과 BigInteger는 크기(mag)를 int배열을 활용하여 엄청나게 긴 binary를 만들어 표현한다. 

```java
public BigInteger(String val, int radix) {
    int cursor = 0, numDigits;
    final int len = val.length();

    if (radix < Character.MIN_RADIX || radix > Character.MAX_RADIX)
        throw new NumberFormatException("Radix out of range");
    if (len == 0)
        throw new NumberFormatException("Zero length BigInteger");

    // Check for at most one leading sign
    int sign = 1;
    int index1 = val.lastIndexOf('-');
    int index2 = val.lastIndexOf('+');
    if ((index1 + index2) <= -1) {
        // No leading sign character or at most one leading sign character
        if (index1 == 0 || index2 == 0) {
            cursor = 1;
            if (len == 1)
                throw new NumberFormatException("Zero length BigInteger");
        }
        if (index1 == 0)
            sign = -1;
    } else
        throw new NumberFormatException("Illegal embedded sign character");

    // Skip leading zeros and compute number of digits in magnitude
    while (cursor < len &&
          Character.digit(val.charAt(cursor), radix) == 0)
        cursor++;
    if (cursor == len) {
        signum = 0;
        mag = ZERO.mag;
        return;
    }

    // 입력된 전체 문자열 길이에서, 부호나 앞 선 0을 제외한 길이를 얻는다.
    numDigits = len - cursor;
    signum = sign;

    // Pre-allocate array of expected size. May be too large but can
    // never be too small. Typically exact.
    // bitsPerDigit[10]은 3402이다. 
    // digitsPerInt[10]은 9이다. 10진수의 경운 9자리씩 끊어서 하나의 int로 저장
    int numBits = (int)(((numDigits * bitsPerDigit[radix]) >>> 10) + 1);
    int numWords = (numBits + 31) >>> 5;
    int[] magnitude = new int[numWords];

    // Process first (potentially short) digit group
    int firstGroupLen = numDigits % digitsPerInt[radix];
    if (firstGroupLen == 0)
        firstGroupLen = digitsPerInt[radix];
    String group = val.substring(cursor, cursor += firstGroupLen);
    magnitude[numWords - 1] = Integer.parseInt(group, radix);
    if (magnitude[numWords - 1] < 0)
        throw new NumberFormatException("Illegal digit");

    // Process remaining digit groups
    int superRadix = intRadix[radix];
    int groupVal = 0;
    while (cursor < len) {
        group = val.substring(cursor, cursor += digitsPerInt[radix]);
        groupVal = Integer.parseInt(group, radix);
        if (groupVal < 0)
            throw new NumberFormatException("Illegal digit");
        destructiveMulAdd(magnitude, superRadix, groupVal);
    }
    // Required for cases where the array was overallocated.
    mag = trustedStripLeadingZeroInts(magnitude);
}
```


## 실질적으로 더하기를 수행하는 내부 메소드
올림(carry)과 관련된 처리를 해주면서 두 int 배열 중 더 긴 길이만큼의 시간복잡도로 연산을 수행한다.

```java
private static int[] add(int[] x, int[] y) {
    // If x is shorter, swap the two arrays
    if (x.length < y.length) {
        int[] tmp = x;
        x = y;
        y = tmp;
    }

    int xIndex = x.length;
    int yIndex = y.length;
    int result[] = new int[xIndex];
    long sum = 0;

    // Add common parts of both numbers
    while(yIndex > 0) {
        sum = (x[--xIndex] & LONG_MASK) +
              (y[--yIndex] & LONG_MASK) + (sum >>> 32);
        result[xIndex] = (int)sum;
    }

    // Copy remainder of longer number while carry propagation is required
    boolean carry = (sum >>> 32 != 0);
    while (xIndex > 0 && carry)
        carry = ((result[--xIndex] = x[xIndex] + 1) == 0);

    // Copy remainder of longer number
    while (xIndex > 0)
        result[--xIndex] = x[xIndex];

    // Grow result if necessary
    if (carry) {
        int bigger[] = new int[result.length + 1];
        System.arraycopy(result, 0, bigger, 1, result.length);
        bigger[0] = 0x01;
        return bigger;
    }
    return result;
}
```

## 배열의 최대 길이
[자바 공식문서](https://docs.oracle.com/javase/8/docs/api/java/math/BigInteger.html)에 따르면 BigInteger는 -2^(2^31)초과 2^(2^31)미만의 범위를 지원해야한다. 이와 관련하여 아래와 같은 구현 사항을 확인할 수 있다.

> 📘 BigInteger constructors and operations throw ArithmeticException when the result is out of the supported range of -2^Integer.MAX_VALUE (exclusive) to +2^Integer.MAX_VALUE (exclusive).

BigInteger로 (2^(2^31) - 1)를 저장한다면 int 배열의 길이는 얼마일까? (2^(2^31) - 1)를 표현하기 위해서는 비트가 2^31개 있어야 한다. 그리고 int 한 개당 31bit를 표현 할 수 있다. 따라서 배열의 길이는 (2^31) / 31인 약 69,273,666까지 늘어날 수 있다. 

이것을 보다 일반화해서 정리한다면, BigInteger int배열의 길이는 ((이진수 표현시 자릿수) / 31) + 1이다. 어떤 수 x의 이진수 표현시 자릿수 y는 x에 대해서 로그 함수이다. 따라서 int배열은 O(log(x))의 공간복잡도를 가지고 있다고 할 수 있다.

위에서 BigInteger a, b의 덧셈은, a,b 중 긴 int배열 만큼의 시간복잡도를 가진다고 했다. 즉, O(max(log(a), log(b)))의 시간복잡도를 가진다. 숫자가 커질수록 연산이 늘어나는 경향을 가지고 있지만, 로그로 늘어나기 때문에 성능이 크게 느리지는 않은 것이다.
