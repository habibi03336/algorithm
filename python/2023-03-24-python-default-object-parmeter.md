---
title: 파이썬, 함수 기본 인자로 객체를 할당할 때 주의할 점 

date: 2023-3-24

categories:
  - python
---

파이썬 함수 파라미터에 기본 인자로 객체를 할당하고, 같은 함수를 여러 번 호출하면 마치 상위 스코프에 객체를 선언하고 참조하는 것처럼 동작한다. 즉, 함수를 호출할 때 매번 새로운 객체를 생성하여 할당하는 것이 아니라, 같은 객체를 재활용한다. 이는 예상치 못한 동작으로 이어질 수 있어서 주의가 필요하다.

```python
def hello(arr=[]):
    arr.append('hello')
    print(arr)

hello() # ['hello']
hello() # ['hello', 'hello']
hello() # ['hello', 'hello', 'hello']
```

파이썬과는 다르게 자바스크립트의 경우 매번 다른 객체를 생성하는 것으로 동작한다.

```javascript
function hello(arr=[]){
    arr.push('hello');
    console.log(arr);
}

hello(); // [ 'hello' ]
hello(); // [ 'hello' ]
hello(); // [ 'hello' ]
```