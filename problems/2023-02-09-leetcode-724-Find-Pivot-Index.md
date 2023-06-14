---
title: LeetCode, 724. Find Pivot Index

date: 2023-2-9

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/find-pivot-index/description/)

정수가 담긴 배열 nums가 주어진다. 왼쪽 요소의 총합과, 오른쪽 요소의 총합을 같게하는 인덱스를 pivot index라고 하자. pivot index는 합에 포함되지 않는다. pivot index를 반환하라.

- 답이 여러 개이면 가장 왼쪽에 있는 pivot index를 반환하여라
- 답이 없으면 -1을 반환하라.

# 문제접근

1. 무식한 방법으로 하면 한 index에 대해서 왼쪽 합과 오른쪽 합을 일일이 매번 구하여 비교하는 방식으로 풀어줄 수 있다. O(n^2)

2. 왼쪽의 합은 누적으로 구할 수 있고, 오른쪽의 합은 총합과 왼쪽 누적, 현재 인덱스를 고려하여 알 수 있다. 이를 활용해 최적화를 해주면 O(n)으로 풀 수 있다.

# 코드

```java
class Solution {
    public int pivotIndex(int[] nums) {
        int total = 0;
        for(int elem : nums){
            total += elem;
        }
        int left = 0;
        for(int i = 0; i < nums.length; i += 1){
            if(left == (total - nums[i] - left)) return i;
            left += nums[i];
        }
        return -1;
    }
}
```
