---
title: LeetCode, 396. Rotate Function

date: 2023-2-27

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/rotate-function/)

정수가 담긴 배열 nums가 주어진다. 이 배열은 k번 째 순서에 대해서 rotate한 후 다음과 같이 배열의 요소를 더한 것을 F(k)라고 한다.

0 \* nums\[0\] + 1 \* nums1. + 2 _ nums2. + ... + (n-1) _ nums\[n-1\]

0 <= k <= nums.length - 1 인 모든 정수 k에 대해서 가장 큰 F(k) 값을 반환하라.

- 1 <= nums.length <= 100,000
- 100 <= nums[i] <= 100

# 문제접근

1. 브루트 포스로 O(n^2)으로 풀 수 있다. n이 100,000이라서 제곱이면 백억이다. 시간초과가 나온다.

2. 앞 서 구해놓은 값을 활용하는 방식을 고려했다. F(k)를 구할 때 k번 째를 기준으로 앞과 뒤를 나눌 수 있다. 0부터 k-1까지, k+1부터 끝까지 두 부분으로 나눌 수 있다. 앞 부분과 뒷 부분을 각각 봐주었더니 기존 값을 활용하여서 k 번째 값을 구해줄 수 있는 것이 보였다. 이 방식은 O(n)이다.

- F(k)의 뒷 쪽 부분은, F(k+1)의 뒷 쪽 부분에 nums\[nums.length-1\] + nums\[nums.length - 2\] + ... + nums\[k + 2\] 를 더 더하고, nums\[k+1\]를 더한 값이다.
- F(k)의 앞 쪽 부분은, F(k-1)의 앞 쪽 부분에 nums\[0\] + nums1. + ... + nums\[k - 2\]를 빼고, nums\[k-1\] \* (nums.length - 1)를 더한 값이다.

3. 훨씬 간단하고 깔끔한 풀이도 있었다. 다음과 같은 방식으로 모든 경우를 고려하게 된다. 이는 시간 복잡도 O(n), 공간복잡도 O(1)의 풀이이다.

1. 처음 시작 값(0, 1, 2, 3) ===> 모든 배열의 요소 값 합 더해줌(1, 2, 3, 4) ===> 마지막 4를 제외(1, 2, 3, 0)

2. 다음 시작 값(1, 2, 3, 0) ===> 모든 배열의 요소 값 합 더해줌(2, 3, 4, 1) ===> 마지막에서 두 번째 4제외 (2, 3, 0, 1)

   ...

# 코드

## 처음 접근한 풀이

```java
class Solution {
    public int maxRotateFunction(int[] nums) {
        int[] cumFromBack = new int[nums.length];
        cumFromBack[nums.length-1] = 0;
        int addOperand = nums[nums.length - 1];
        for(int i = nums.length-2; i >= 0; i -= 1){
            cumFromBack[i] = cumFromBack[i+1] + addOperand;
            addOperand += nums[i];
        }
        int[] cumFromFront = new int[nums.length];
        cumFromFront[0] = 0;
        int minusOperand = 0;
        for(int i = 1; i < nums.length; i += 1){
            cumFromFront[i] += nums[i-1] * (nums.length - 1) + cumFromFront[i-1] - minusOperand;
            minusOperand += nums[i-1];
        }
        int res = - Integer.MAX_VALUE;
        for(int i = 0; i < nums.length; i += 1){
            res = Math.max(res, cumFromBack[i] + cumFromFront[i]);
        }
        return res;
    }
}
```

## 깔끔한 풀이

```java
class Solution {
    public int maxRotateFunction(int[] nums) {
        int F = 0;
        int S = 0;
        for(int i = 0; i < nums.length; i++){
            F = F + (nums[i] * i);
            S = S + nums[i];
        }
        int max = F;
        int n = nums.length;
        for(int i = n - 1; i >= 1 ; i--){
            F = F + S - n * nums[i];
            max = Math.max(max , F);
        }
      return max;
    }
}
```
