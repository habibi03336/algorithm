---
title: 자바스크립트, 배열을 정렬하는 방법
date: 2022-9-23
categories:
  - javascript
tags:
  - TIL
---

# 기본 문법

## 기존 배열을 정렬

```javascript
const array = [3, 2, 1];
array.sort();
console.log(array); // [1, 2, 3]
```
## 기존 배열에 대한 부수효과 없이 정렬된 결과 생성

```javascript
const array = [3, 2, 1];
const array2 = [...array];
array2.sort();
console.log(array); // [3, 2, 1]
console.log(array2); // [1, 2, 3]
```

# Compare function

자바스크립트의 sort() 메소드는 기본적으로 내부의 모든 요소를 string으로 cast하여 문자열 끼리 비교하여 오름차순으로 정렬한다. 문자열을 비교할 때는 맨 앞 문자부터 유니코드로 비교한다. (숫자 배열을 정렬할 때 이 점을 간과하기 쉬우므로 조심하자!)

```javascript
const array = [1, 2, 3, 10, 100, 12];
array.sort();
console.log(array); // [1, 10, 100, 12, 2, 3]
```

정렬 기준을 바꾸기 위해서는 sort 메소드 첫 번 째 인자로 compare function을 넣어주면 된다.

```typescript
type compareFunction = (a, b) => number;
```

1. 반환 값이 0보다 작으면 a를 b보다 앞 쪽(색인이 낮은 쪽)으로 정렬한다.  
2. 반환 값이 0보다 크면 a를 b보다 뒷 쪽(색인이 높은 쪽)으로 정렬한다.  
3. 반환 값이 0이면 기존 배열의 순서를 유지한다.

## Number 정렬하기

```javascript
const array = [1, 2, 3, 10, 100, 12];
array.sort((a, b) => a - b); // 오름차순 정렬
console.log(array); // [ 1, 2, 3, 10, 12, 100 ]

array.sort((a, b) => b - a); // 내림차순 정렬
console.log(array); // [ 100, 12, 10, 3, 2, 1 ]
```

## 객체의 값에 따라 객체의 키를 정렬하기

```javascript
const foodCounts = {
  라면: 2,
  수제비: 3,
  김밥: 1,
  아이스크림: 0,
  장조림: 4,
};

const foodNames = Object.keys(foodCounts);
foodNames.sort((a, b) => foodCounts[a] - foodCounts[b]);
console.log(foodNames); // [ '아이스크림', '김밥', '라면', '수제비', '장조림' ]
```

---

__참고__
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort
