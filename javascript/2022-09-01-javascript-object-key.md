---

title: 자바스크립트, 객체의 key를 사용하여 비교연산 할 때 주의할 점
date: 2022-9-1
categories: 
    - javascript
    
---

자바스크립트는 동적타이핑을 하는 언어이다. 또한 암묵적 변환을 적극적으로 하는 프로그래밍 언어이기도 하다. 이는 편리할 때도 있지만 때떄로 예상치 못한 에러를 내기도 한다. 객체의 key를 활용하여 프로그래밍 할 때 이와 관련하여 주의할 점이 있다.  

Number로 구성된 Array의 요소를 object를 활용하여 count한다고 하면 다음과 같은 코드를 짤 수 있다.  
```javascript
const numberArray = [5, 4, 7, 2, 1, 4, 8, 7, 5, 2];
const count = {};
numberArray.forEach((elem) => {
    if(count[elem]) {
        count[elem] = 1;
    } else {
        count[elem] += 1;
    }
})
```

이후에 count 객체의 key 값을 활용하여 추가적인 작업을 하고 싶을 수 있다. 이때 주의할 점이 있다. 객체 key data type이 string이라는 점이다. key로 number를 넣었지만 자바스크립트가 string으로 암묵적 변환을 한 것이다. 자바스크립트의 객체는 키로 string이나 symbol 타입만을 가질 수 있기 때문이다(일반적인 사용에서는 string만 쓰인다고 보면 됨).   

따라서 이를 고려하지 않고 비교 연산자를 쓰면 예상과 다르게 작동할 수 있다. 따라서 원하는 타입으로 적절하게 다시 캐스팅 해주는 것이 필요하다.  
```javascript
Object.entries(count).forEach(([key, val])=>{
    console.log(typeof key); //string 

    if(key === 5){
        console.log("hello"); // 실행 안됨 
    }

    if(Number(key) === 5){
        console.log("hello"); // 실행 됨
    }
})
```  

