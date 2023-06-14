---
title: LeetCode, 13. Roman to Integer

date: 2023-2-5

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/roman-to-integer/description/)

로마 숫자가 주어진다. 로마 숫자를 정수로 변환하라.

- 유효한 로마 숫자만 주어진다.
- 변환된 값은 1에서 3999 사이이다.

# 문제접근

1. 로마 숫자에 매핑되는 정수를 모두 더 해준다. 다만 더하는 과정에서 로마숫자가 뒤의 문자에 대한 뺄셈을 의미하는 경우(문자 순서로 판단 가능) 다시 빼준다.

# 코드

```java
class Solution {
    public int romanToInt(String s) {
       HashMap<Character, Integer> ht = new HashMap<Character, Integer>();
       ht.put('I', 1);
       ht.put('V', 5);
       ht.put('X', 10);
       ht.put('L', 50);
       ht.put('C', 100);
       ht.put('D', 500);
       ht.put('M', 1000);
       int res = 0;
       Character prev = null;
       for(int i = 0; i < s.length(); i += 1){
           Character curr = s.charAt(i);
           res += ht.get(curr);
           if(
               prev != null &&
               ((prev == 'I' && (curr == 'V' || curr == 'X')) ||
               (prev == 'X' && (curr == 'L' || curr == 'C')) ||
               (prev == 'C' && (curr == 'D' || curr == 'M')))
            ){
               res -= 2 * ht.get(prev);
            }
            prev = curr;
       }
       return res;
    }
}
```
