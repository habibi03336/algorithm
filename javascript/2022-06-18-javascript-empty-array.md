---

title: 자바스크립트, 빈 배열 생성하는 방법 및 주의사항
date: 2022-6-18
categories: 
    - javascript

---
### 자바스크립트에서 Array 만들기

데이터를 처리하다보면 count를 할 때나 중간 데이터를 저장할 때 빈 배열을 활용하는 경우가 많다. 
자바스크립트로 빈 배열을 생성하는 방법은 다음과 같다.

```javascript
const n = 10; // array size
const emptyArray = Array(n).fill();
```
---

Array(n)만으로 충분하지 않은 이유는 Array(n)은 length 프로퍼티만 n으로 설정한 object를 return하기 때문이다. fill()을 하면 length에 따라 0,1,2,3 ... n-1 프로퍼티를 만들고 undefined로 초기화 한다. map과 같은 array 메쏘드가 기대처럼 동작하려면 fill()로 프로퍼티 키를 만들고 초기화 해줘야한다.

```javascript
Object.keys(Array(4)); // []
Object.keys(Array(4).fill()); // [ '0', '1', '2', '3' ]
```

### 객체로 초기화 하고자 할 때 주의사항 [update: 2022.9.16]
때때로 2차원 배열 등 객체 배열로 초기화 하고 싶을 수가 있다. 이때 주의할 것은 다음과 같이 하면 안 된다는 것이다.

```javascript
const array2D = Array(3).fill([]);
array2D[0].push("hello");
console.log(array2D); // [['hello'], ['hello'], ['hello']]
``` 

첫 번째 배열에만 "hello" string을 push 했지만 모든 배열에 "hello"가 추가된 것처럼 작동한다. 이는 각 배열의 요소가 서로 다른 array로 초기화 된 것이 아니고, 같은 array의 레퍼런스로 초기화 되었기 때문이다. 이는 의도한 바가 아니므로 이처럼 초기화하면 안 된다.

적절한 방식으로는 다음과 같은 것이 있을 수 있다.

```javascript
const array2D = Array(3).fill().map(()=>[]);
array2D[0].push("hello");
console.log(array2D); // [ [ 'hello' ], [], [] ]
```