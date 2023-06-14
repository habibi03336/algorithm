---
title: LeetCode, 7. Reverse Integer

date: 2023-2-5

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/reverse-integer/description/)

int가 주어진다. int의 숫자를 반대로 뒤집은 int 값을 반환하라. 만약 반대로 뒤집은 수가 int의 범위를 초과하면 0을 반환하라.

- 64bit 정수형(long)이 없다고 가정한다.
- x의 범위: [-2^31, 2^31 - 1]

# 문제접근

1. 32bit 정수형만 있다는 것이 이 문제의 특징이다. 오버플로우가 나지 않으면서도 비교를 할 수 있어야 한다.

- 범위를 넘어서는지 확인해주기 위해서 가장 큰 값(2^31-1)의 자릿수를 배열로 저장하여 비교해주었다.
- 정수형의 경우 음수인 경우가 절대값이 1더 큰 수를 표현할 수 있다. 2의 보수로 값을 나타내기 때문이다. 이를 별도로 예외처리 해주었다.

# 코드

```java
class Solution {
    public int reverse(int x) {
        if(x == Integer.MIN_VALUE) return 0;
        ArrayList<Integer> maxNums = new ArrayList<Integer>();
        int max = Integer.MAX_VALUE;
        while(max != 0){
            int n = max % 10;
            max = max / 10;
            maxNums.add(n);
        }
        ArrayList<Integer> nums = new ArrayList<Integer>();
        boolean isNegative = false;
        if(x < 0) {
            x = -x;
            isNegative = true;
        }
        while(x != 0){
            int n = x % 10;
            x = x / 10;
            nums.add(n);
        }
        if(nums.size() == maxNums.size()){
            int lastIndex = maxNums.size() - 1;
            for(int i = 0; i <= lastIndex; i += 1){
                if(nums.get(i) > maxNums.get(lastIndex - i)){
                    return 0;
                } else if (nums.get(i) < maxNums.get(lastIndex - i)){
                    break;
                }
            }
        }
        int res = 0;
        int lastIndex = nums.size() - 1;
        for(int i = 0; i <= lastIndex; i += 1){
            res += nums.get(i) * Math.pow(10, lastIndex - i);
        }
        return isNegative ? -res : res;
    }
}
```
