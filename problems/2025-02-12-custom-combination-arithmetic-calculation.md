---
title: 커스텀, 산술 표현 계산 with 괄호 경우의 수 

date: 2025-2-12

categories:
  - 문제풀이

tags:
---

# 문제요약

1 + 3 - 10 + 7 -2 - 6

"1+3-5\*2+7-1\*2-6"과 같은 산술 표현(arithmetic expression)이 주어진다. 산술 표현은 정수 및 +,-,\*으로만 이루어져있다.
산술 표현에 괄호 한 쌍을 추가하여 산술 표현의 계산 순서를 바꿀 수 있다. 어떤 산술 표현이 주어지고, 한 쌍의 괄호를 추가하여 계산할 수 있을 때 가장 큰 계산의 결과를 반환하라.

- 산술 표현의 길이는 3 이상 101 이하이다.
- 계산 결과 값 및 중간 계산 결과 값은 -(10^15) 이상 10^15 이하이다.

# 문제접근

1. 산술 표현의 길이가 101이하이기 때문에 총 산술량은 최대 50번이다. 51개의 숫자에 대해 50번 산술을 하는 산술 표현이 101의 길이를 가지게된다. 따라서 괄호를 넣을 수 있는 최대 경우의 수는 52 콤비네이션 2 이다.

2. 산술 표현을 계산하는데 산술 표현의 길이 만큼의 시간이 필요하다. 문자열을 순차적으로 읽으면서, 스택을 활용하여 선형시간으로 계산을 해낼 수 있다.

3. 1,2의 종합하여 보면 최대 계산량은 대략 (52 콤비네이션 2) \* 101 이기 때문에 충분히 브루트 포스로 계산할 수 있다.

- 계산값 조건에 따라 long 타입을 사용하면 안전하다. (파이썬에서는 오버플로우가 자동으로 관리됨으로 고려하지 않아도 된다.)

# 코드

```python
import re
import sys

input_expression = input().strip()
expression = []

operator = ['*', '+', '-']
token = []
for i in range(len(input_expression)):
  if input_expression[i] in operator:
    expression.append(''.join(token))
    expression.append(input_expression[i])
    token = []
  else:
    token.append(input_expression[i])
expression.append(''.join(token))

def calculate(expression, start=0, end=None):
  stack = []
  i = start
  end = len(expression) if end is None else end
  while i < end:
    token = expression[i]
    if token == '-':
      stack.append(-int(expression[i+1]))
      i += 2
    elif token == '*':
      a = stack.pop()
      b = int(expression[i+1])
      stack.append(a*b)
      i += 2
    elif token == '+':
      i += 1
      pass
    else:
      stack.append(int(token))
      i += 1
  return sum(stack)

def solution(expression):
  end = len(expression)
  max_res = calculate(expression)

  for i in range(0, end-2, 2):
    for j in range(i+2, end, 2):
      b = calculate(expression, i, j+1)
      expr = expression[0:i] + [b] + expression[j+1:]
      max_res = max(max_res, calculate(expr))
  return max_res

print(solution(expression))
```
