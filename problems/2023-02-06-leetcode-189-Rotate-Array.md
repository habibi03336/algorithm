---
title: LeetCode, 189. Rotate Array

date: 2023-2-6

categories:
  - 문제풀이

tags:
  - Exercise
  - 🧑🏻‍💻
---

# [문제요약](https://leetcode.com/problems/rotate-array/description/)

정수 배열 nums가 주어진다. 음이 아닌 정수 k가 주어질 때 배열을 오른쪽으로 k만큼 돌려라.

> Input: nums = [1,2,3,4,5,6,7], k = 3  
> Output: [5,6,7,1,2,3,4]

- 1 <= nums.length <= 100,000
- 0 <= k <= 100,000

# 문제접근

1. 앞쪽으로 옮길 k만큼을 임시로 저장한다. 나머지를 k만큼 뒤로 옮겨주고 임시로 저장한 것으로 앞을 채워준다.

- k가 nums.length보다 큰 경우를 고려해주어야 한다. k가 nums.length보다 큰 경우 한 바퀴를 이상을 완전히 돈다는 의미이므로 위치에 영향을 주는 않는다. 따라서 그만큼을 빼준다.
- k가 0인 경우 바로 함수를 종료시킨다.

2. O(1) 공간복잡도 + 효율적인 알고리즘이 무엇이 있을까?

# 코드

```java
class Solution {
    public void rotate(int[] nums, int k) {
        while(nums.length <= k){
            k -= nums.length;
        }
        if(k == 0) return;
        int[] tmp = new int[k];
        for(int i = k; i > 0; i -= 1){
            tmp[k - i] = nums[nums.length - i];
        }
        for(int i = nums.length - k - 1; i >= 0; i -= 1){
            nums[i+k] = nums[i];
        }
        for(int i = 0; i < k; i += 1){
            nums[i] = tmp[i];
        }
    }
}
```
