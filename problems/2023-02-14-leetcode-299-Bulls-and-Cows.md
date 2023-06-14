---
title: LeetCode, 299. Bulls and Cows

date: 2023-2-14

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/bulls-and-cows/description/)

bulls anc cows 게임을 한다(숫자 야구 게임과 같다). 맞춰야하는 숫자열 secret이 있고, 예상값 guess가 주어진다. 만약 숫자와 자리가 모두 같으면 bull이고, 자리는 다른데 숫자만 같은게 있으면 cow이다. bull 개수와 cow 개수를 '(bull개수)A(cow개수)B' 형식으로 반환하라.

- bull로 카운팅 한 것은 cow로 다시 count하지 않는다.
- secret.length == guess.length 이다.

# 문제접근

1. 두 문자열을 선형적으로 비교하여 bullCount를 구할 수 있다. secret의 문자를 카운팅 해놓고 guess에 있으면 하나씩 빼는 방식으로 cowCount를 구할 수 있다. 중복으로 세지 않는다고 했으므로 마지막 결과에서는 cowCount에서 bullCount를 빼준다.

# 최적화

1. 문자가 숫자로 제한되므로 hashmap을 쓰지 않고 배열로 카운팅 해줄 수 있다. (문자 '0'은 char로 48이다.) hash의 오버헤드를 없앨 수 있다.

# 코드

## 배열 활용 카운팅

6 ms, 42.2 MB

```java
class Solution {
    public String getHint(String secret, String guess) {
        int[] digitCount = new int[10];
        int bullCount = 0;
        for(int i = 0; i < secret.length(); i += 1){
            char c = secret.charAt(i);
            digitCount[c-48] += 1;
            if(c == guess.charAt(i)){
                bullCount += 1;
            }
        }
        int cowCount = 0;
        for(int i = 0; i < guess.length(); i += 1){
            char c = guess.charAt(i);
            if(digitCount[c-48] != 0){
                cowCount += 1;
                digitCount[c-48] -= 1;
            }
        }
        return String.format("%dA%dB", bullCount, cowCount - bullCount);
    }
}
```

## hashmap 활용 카운팅

8 ms, 42.2 MB

```java
class Solution {
    public String getHint(String secret, String guess) {
        Map<Character, Integer> charCount = new HashMap<>();
        int bullCount = 0;
        for(int i = 0; i < secret.length(); i += 1){
            char c = secret.charAt(i);
            if(charCount.containsKey(c)){
                charCount.put(c, charCount.get(c) + 1);
            } else {
                charCount.put(c, 1);
            }
            if(c == guess.charAt(i)){
                bullCount += 1;
            }
        }
        int cowCount = 0;
        for(int i = 0; i < guess.length(); i += 1){
            char c = guess.charAt(i);
            if(charCount.containsKey(c) && charCount.get(c) != 0){
                cowCount += 1;
                charCount.put(c, charCount.get(c) - 1);
            }
        }
        return String.format("%dA%dB", bullCount, cowCount - bullCount);
    }
}
```
