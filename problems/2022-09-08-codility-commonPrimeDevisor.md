---
title: Codility, Common Prime Devisor

date: 2022-9-8

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제요약

어떤 수가 있을 때, 그 수를 나누어 떨어지게 하는 소수를 prime devisor라고 한다. 두 배열이 주어지고 같은 색인의 값을 하나의 쌍으로 봤을 때, 몇 개의 쌍이 동일한 prime devisor를 가지고 있는지를 count하여라.

# 문제접근.

모든 양수는 소수로 분해될 수 있다. 한 수를 고정시켜놓고 gcd를 구해서 그 수를 gcd로 계속 나누는 방식으로 그 수만 가지고 있던 소수가 있는지 확인할 수 있다.

# 시행착오

[실패한 풀이(61%)](https://app.codility.com/demo/results/training2ZP5U8-K4K/)

큰 숫자 기준으로만 확인을 하면 된다고 생각했는데 그렇지가 않았다. 크기와 관계 없이 상대와의 gcd로 모든 소수가 소거될 수 있는지 확인해 줘야한다.

# [풀어냄!](https://app.codility.com/demo/results/trainingGE963Z-A38/)

```javascript
function solution(A, B) {
    const findGCD = (a, b) => {
        if(a % b === 0) return b;
        else return findGCD(b, a % b);
    }

    let cnt = 0;

    for(let i = 0; i < A.length; i++){
        const numA = A[i] 
        const numB = B[i]

        let num = numA;
        let gcd = findGCD(numB, num);
        while(gcd !== 1){
            num /= gcd;
            gcd = findGCD(numB, num);
        }

        let num2 = numB;
        gcd = findGCD(numA, num2);
        while(gcd !== 1){
            num2 /= gcd;
            gcd = findGCD(numA, num2);
        }


        if(num === 1 && num2 === 1){
            cnt++;
        }
    }

    return cnt;
}
```
