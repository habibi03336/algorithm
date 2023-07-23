---
title: LeetCode, 9. Palindrome Number

date: 2023-2-3

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/palindrome-number/description/)

정수가 주어진다. 정수가 palindrome인지 판별하라.

- 음수인 경우는 (-10 => 01-) palindrome이 아니다.
- 10을 거꾸로 읽으면 1이므로 palindrome이 아니다.

# 문제접근

1. 자리 수 별로 비교해주기 위해서 문자열로 casting하고 포인터를 활용하여 비교해준다.

- 음수인 경우, 첫 번쨰 자리가 0인 경우에 예외처리가 필요해보일 수도 있지만, 맨 앞에 - 있는경우, 맨 뒤에 0이 있는 경우 모두 반대쪽과 같을 수가 없기 때문에 자동으로 예외처리가 된다.

# 코드

```java
class Solution {
    public boolean isPalindrome(int x) {
        String nums = String.valueOf(x);
        int i = 0;
        int j = nums.length() - 1;
        while(i <= j){
            if(nums.charAt(i) != nums.charAt(j)){
                return false;
            }
            i += 1;
            j -= 1;
        }
        return true;
    }
}
```
