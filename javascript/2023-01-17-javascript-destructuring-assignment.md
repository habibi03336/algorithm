---
title: 자바스크립트, 구조 분해 할당
date: 2022-6-17
categories:
  - javascript
---

# 자바스크립트에서는 구조 분해 할당이란 기능을 제공한다.

## 배열의 구조 분해 할당

```javascript
const arr = [1, 2, 3];
const [a, b, c] = arr;
// a : 1, b: 2, c:3이 할당된다.
```

## 객체의 구조 분해 할당

```javascript
const obj = { a: 1, b: 2, c: 3 };
const { a, b, c } = obj;
// a : 1, b: 2, c:3이 할당된다.
```

## 구조 분해 할당의 실행순서

구조 분해 할당은 다음과 같이 실행된다.

```javascript
const arr = [1, 2, 3];
// const [a, b, c] = arr;
const tmp0 = arr[0];
const tmp1 = arr[1];
const tmp2 = arr[2];
const a = tmp0;
const b = tmp1;
const c = tmp2;
// a : 1, b: 2, c:3이 할당된다.
```

주의해야할 점은 순서에 영향을 받는다는 점이다.

```javascript
let a = { type: "gold" };
let b = { type: "silver" };
```

이라고 해보자

```javascript
[a, a.type, b] = [b, b.type, a];
// a: {type: "silver}, b: {type: "gold"}
```

```javascript
[a.type, a, b] = [b.type, b, a];
// a: {type: "silver}, b: {type: "silver"}
```

다음과 같이 결과가 다르다. 이는 실행순서와 관련이 있다.

```javascript
const tmpB = b;
const tmpBType = b.type;
const tmpA = a;
a = tmpB;
a.type = tmpBType;
b = tmpA;
```

```javascript
const tmpBType = b.type;
const tmpB = b;
const tmpA = a;
a.type = tmpBType;
a = tmpB;
b = tmpA;
```

즉 할당 대상은 미리 평가되어 값이 고정되지만, 할당하는 순서는 영향을 준다.
