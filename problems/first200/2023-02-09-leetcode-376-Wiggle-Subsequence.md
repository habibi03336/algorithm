---
title: LeetCode, 376. Wiggle Subsequence

date: 2023-2-9

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/wiggle-subsequence/description/)

연속적인 배열 요소의 차이가 양과 음을 왔다갔다하는 경우를 wiggle suquence라고 한다. 정수가 담긴 배열 nums가 주어질 때, 배열의 가장 긴 wiggle subsquence의 길이를 반환하라.

- subsquence는 배열에서 일부 요소를 제외하고, 남은 요소는 기존 순서대로 놓여있는 것을 말한다. (sequence 자체도 subsequence이다)

# 문제접근

1. 무식한 방법으로, 모든 subsequence의 경우의 수(O(2^n))에 대해서 wiggle sequence를 만족하는 가장 긴 subsquence를 찾아 줄 수 있다. 모든 subsquence를 생성하는 것부터 엄청난 일이 되어버린다.

2. 그리디한 방식으로 접근할 수 있다는 것을 발견했다. 그리디한 선택이 뒤 이은 문제에 대한 최적의 선택이기 때문이다.

# 코드

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        int len = 1;
        int p = 0;
        while(p + 1 < nums.length){
            if(nums[p] == nums[p+1]){
                p += 1;
            } else if (nums[p] < nums[p+1]) {
                while(p + 1 < nums.length && nums[p] <= nums[p+1]){
                    p += 1;
                }
                len += 1;
            } else if(nums[p] > nums[p+1]) {
                while(p + 1 < nums.length && nums[p] >= nums[p+1]){
                    p += 1;
                }
                len += 1;
            }
        }
        return len;
    }
}
```
