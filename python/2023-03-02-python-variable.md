---
title: 파이썬, 변수

date: 2023-3-2

categories:
  - python
---

파이썬에는 변수 선언 키워드가 없다. 따라서 선언과 할당을 구분할 수 없다. 다음과 같이 파이썬 함수 내부에서 외부에 있는 변수 이름을 사용하면 함수 내부 범위에서 새로운 변수를 선언한 것으로 동작한다.
때때로 외부의 변수를 그대로 쓰고 싶을 수 있다 이 때 global 키워드를 활용할 수 있다.

```python
word = "world"

def test():
  word = "hello"
  print(word)

test() # "hello"
print(word) # "world"
```

```python
word = "world"

def test():
  global word
  word = "hello"
  print(word)

test() # "hello"
print(word) # "hello"
```

만약 같은 변수 이름을 쓰는 함수가 겹쳐있고 global을 쓰는 경우는 어떻게 될까? 다음 코드를 통해 가장 상위의 변수를 참조하는 것을 확인할 수 있다.

```python
word = "hello"

def test():
  word = "world"

  def nested():
    global word
    print(word)

  nested()

test() # "hello"
```

### 특이한 점

가변객체의 데이터를 바꿀 때는 global 키워드 없이 외부 변수를 그대로 활용할 수 있다. 가변객체의 경우 선언과 데이터 변경이 명확히 구분되기 때문에 이렇게 동작하는 것이 이해는 된다.

```python
cnt_arr = [1]
test_dict = { "a" : 1}

def test():
  cnt_arr[0] += 1
  test_dict["a"] += 1

test()
test()

print(cnt_arr[0]) # 3
print(test_dict["a"]) # 3
```
