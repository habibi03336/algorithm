---
title: LeetCode, 605. Can Place Flowers

date: 2023-2-10

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/can-place-flowers/description/)

꽃밭을 나타내는 정수 배열 flowerbed가 주어진다. 0은 꽃이 심기지 않은 것, 1은 꽃이 심긴 것을 나타낸다. 또한 추가로 심을 꽃의 개수 n이 주어진다. 꽃을 연속적으로 심을 수 없다고 할 때, 추가적으로 n 송이의 꽃을 심을 수 있는지 판단하라. 가능하면 true 불가능하면 false를 반환하라.

- 1 <= flowerbed.length <= 20,000
- flowerbed에 연속적으로 심긴 꽃은 없다.
- 0 <= n <= flowerbed.length

# 문제접근

1. 그리디하게 앞에서부터 최대한 뺵뺵하게 심는다고 가정하고, n 송이를 심을 수 있는지 확인해주면 된다. 이미 꽃이 심긴 주변을 제외하고 남은 부분에 대해서 최대한 앞쪽에서부터 심는 것이 최적화된 선택이다.

2. 앞 쪽과 뒷 쪽을 조회하여 판단하기 때문에 좀 더 예외처리에 신경을 써줘야 했다. outofindex 에러가 발생하기 좋은 문제이다.

# 코드

```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        if(n == 0) return true;
        if(flowerbed.length == 1 && flowerbed[0] != 0 && n != 0) return false;
        if(flowerbed.length == 1 && flowerbed[0] == 0) return true;
        if(flowerbed[0] == 0 && flowerbed[1] == 0) {
            flowerbed[0] = 1;
            n -= 1;
        }
        for(int i = 1; i < flowerbed.length - 1; i += 1){
            if(flowerbed[i-1] == 0 && flowerbed[i] == 0 && flowerbed[i+1] == 0){
                flowerbed[i] = 1;
                n -= 1;
            }
        }
        if(flowerbed[flowerbed.length-2] == 0 && flowerbed[flowerbed.length-1] == 0){
            flowerbed[flowerbed.length - 1] = 1;
            n -= 1;
        }
        return n <= 0 ? true : false;
    }
}
```
