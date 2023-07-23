---
title: LeetCode, 645. Set Mismatch

date: 2023-2-8

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/set-mismatch/description/)

1부터 n까지의 정수가 담긴 집합 s가 있었다. 에러로 인해 이중 하나가 s의 다른 정수로 바뀌었다. 에러가 난 후의 집합을 배열로 나타낸 nums가 주어졌을 때, 중복되는 정수와 그 정수의 원래 값을 반환하라.

# 문제접근

1. 배열을 이용하여 정수를 카운팅해주는 방식으로 빠진 정수(배열의 값이 0)와 중복된 정수(배열의 값이 2)를 찾아 줄 수 있다. O(N)

# 코드

```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        int[] record = new int[nums.length + 1];
        Arrays.fill(record, 0);
        for(int i = 0; i < nums.length; i += 1){
            record[nums[i]] += 1;
        }
        int duplicate = 0;
        int missing = 0;
        for(int i = 1; i < nums.length + 1; i += 1){
            if(record[i] == 0) missing = i;
            if(record[i] == 2) duplicate = i;
        }
        return new int[]{duplicate, missing};
    }
}
```
