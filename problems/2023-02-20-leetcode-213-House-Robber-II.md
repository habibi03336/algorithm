---
title: LeetCode, 213. House Robber II

date: 2023-2-20

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/house-robber-ii/description/)

0 이상의 정수가 담긴 배열 nums가 있다. nums는 도둑질 할 집에 있는 돈의 양을 나타낸다. 집은 원형으로 배치되어있다. 즉, 가장 마지막 요소와 가장 첫 요소에 대한 집은 붙어있다. 연속적으로 붙어있는 집을 털 경우 알람이 작동해 잡힌다. 연속적으로 붙어있는 집을 털지 않고 훔칠 수 있는 돈의 최대를 반환하라.

- 1 <= nums.length <= 100
- 0 <= nums\[i\] <= 1000

# 문제접근

1. dp의 메모이제이션을 활용하여 효율적으로 문제를 풀 수 있다. 선형적이라고 생각했을 때 i까지 털 수 있는 최대 양은, `(i-2 까지 털 수 있는 최대) + (i번째 집의 돈)`이다.

2. 이 문제는 집이 원형으로 배치되어있다는 조건이 있다. 따라서 마지막 집과 첫 번째 집을 동시에 털 수 없다. 마지막 집과 첫 번째 집을 동시에 터는 것이 최대값을 구한 것인지 확인하기 위해서 첫 번째 집의 돈을 0으로 바꾸고 한 번 더 최대 털 수 있는 양을 구한다. 이것이 기존의 것과 같으면 첫 번째 집이 훔치는 집에 포함되지 않는 다는 의미이고 다르면 그 반대이다.

- 첫 번째 집에 훔치는 집에 포함되지 않는다면, 집이 일렬로 배치된 것과 같으므로 기존 최대 값을 반환하면 된다.
- 첫 번째 집이 훔치는 집에 포함된다면, 두 가지 경우로 나뉜다.
  - 첫 번째 집을 훔치고, 마지막 집을 훔치지 않는 경우 (코드 상: priorMaxRobbery)
  - 첫 번쨰 집을 훔치지 않고, 마지막 집을 훔치는 경우 (코드 상: maxRobberyWithFirstZero)

# 코드

```java
class Solution {
    public int rob(int[] nums) {
        if(nums.length == 1) return nums[0];
        int priorMaxRobbery = 0;
        int maxRobbery = 0;
        for(int i = 0; i < nums.length; i += 1){
            int tmp = maxRobbery;
            maxRobbery = Math.max(maxRobbery, priorMaxRobbery + nums[i]);
            priorMaxRobbery = tmp;
        }
        int firstElem = nums[0];
        nums[0] = 0;
        int priorMaxRobberyWithFirstZero = 0;
        int maxRobberyWithFirstZero = 0;
        for(int i = 0; i < nums.length; i += 1){
            int tmp = maxRobberyWithFirstZero;
            maxRobberyWithFirstZero = Math.max(maxRobberyWithFirstZero, priorMaxRobberyWithFirstZero + nums[i]);
            priorMaxRobberyWithFirstZero = tmp;
        }
        if(maxRobbery == maxRobberyWithFirstZero) return maxRobbery;
        return Math.max(priorMaxRobbery, maxRobberyWithFirstZero);
    }
}
```
