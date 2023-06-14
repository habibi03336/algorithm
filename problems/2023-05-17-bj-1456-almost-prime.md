---
title: 백준, 1456. 거의 소수

date: 2023-5-17

categories:
  - 문제풀이

tags:
---

# [문제요약](https://www.acmicpc.net/problem/1456)

어떤 수가 소수의 N제곱수이면 `거의 소수`라고 한다.
A보다 크거나 같고, B보다 작거나 같은 거의 소수가 몇개인지 출력하라.

- N은 2이상
- A는 1이상 B 이하.
- B는 A이상 10^14 이하.

# 문제접근

1.  B는 최대 10^14(백조)이다. 거의 소수의 base가 되는 소수 (a^b에서 a)는 최대 sqrt(B)이므로 10^7(천만)이다.

2.  2부터 최대 10^7까지를 반복하면서, 소수를 찾아주어야한다. 반복문을 활용해 x가 소수인지 판단하는 것은 시간복잡도가 O(sqrt(x))이다. 따라서 문제 조건상 시간초과가 난다.

        O(sqrt(2) + sqrt(3) + sqrt(4) + ... + sqrt(N)) = O(N*sqrt(N))

<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/habibi03336/algorithm/master/assets/img/sum-sqrt-nums.jpeg" alt="sum-sqrt-nums" width="500"/>
</div>

3. 에라토스테네스의 채를 활용하면 시간복잡도를 개선할 수 있다. 1부터 N까지의 소수를 판단하는데 에라토스테네스의 채의 시간복잡도는 O(N\*log(log(N)))이다.

4. B는 10^14까지이다. 가장 작은 소수 2를 base로 해도 대략 2^50보다 작으므로, 제곱수를 최대 50번만 확인하면 된다. 따라서 거의 상수라고 볼 수 있다.

5. 따라서 전체 시간복잡도는 O(N\*log(log(N)))이다.

# 구현

1.  정수형 오버플로우 이슈 때문에 삽질을 좀 했다. sqrt(B)를 세 번 곱하는 경우가 있는데 이 경우 long의 범위를 한 참 벗어난다.(경 단위도 넘어간다.) 따라서 BigInteger를 활용해서 해결했다.

        A를 1 B를 10^14로 했을 때 long 오버플로우가 나는 경우 670126가 나오고, BigInteger를 활용하여 올바른 답을 구하면 670121가 나온다.
        답이 큰 차이가 나지않고, 에러가 발생하는 것도 아니라서 세심하게 신경쓰지 않으면 찾기가 다소 까다롭다.

# 코드

```java
import java.io.IOException;
import java.util.Scanner;
import java.math.BigInteger;

public class bj_1456 {
    public static void main(String[] args) throws IOException {
        //input
        Scanner scanner = new Scanner(System.in);
        long a = scanner.nextLong();
        long b = scanner.nextLong();

        //output
        System.out.println(Integer.toString(solution(a, b)));
    }


    private static int solution(long a, long b){
        initializePrimeFilter(b);
        int count = 0;
        BigInteger bigB = new BigInteger(Long.toString(b));
        BigInteger bigA = new BigInteger(Long.toString(a));
        for(long i = 2; i * i <= b; i += 1){
            if(isPrime((int)i)){
                BigInteger bigI = new BigInteger(Long.toString(i));
                BigInteger cal = bigI.multiply(bigI);
                while(true){
                    if(cal.compareTo(bigB) == 1){
                        break;
                    }
                    if(cal.compareTo(bigA) >= 0){
                        count += 1;
                    }
                    cal = cal.multiply(bigI);
                }
            }
        }
        return count;
    }

    private static boolean[] filter;

    private static void initializePrimeFilter(long bound){
        filter = new boolean[(int)Math.sqrt(bound) + 1];
    }
    private static boolean isPrime(int n){
        if(n == 1) return false;
        if(filter[n]) return false;
        int num = n + n;
        while(true){
            if(num >= filter.length){
                break;
            }
            filter[num] = true;
            num += n;
        }
        return true;
    }
}
```
