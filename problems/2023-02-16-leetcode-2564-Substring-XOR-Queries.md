---
title: LeetCode, 2564. Substring XOR Queries

date: 2023-2-16

categories:
  - 문제풀이

tags:
  - Exercise
  - 🧑🏻‍💻
---

# [문제요약](https://leetcode.com/problems/substring-xor-queries/)

이진수를 담은 문자열 s와 이에 대한 질의인 queries가 주어진다. qeuries의 요소는 길이가 2인 정수 배열이다. queries\[i\]\[0\]를 xor 연산하였을 때 queries\[i\]1.를 만족하는 s의 부분 문자열의 처음과 끝 인덱스를 찾아라. 만약 없으면 [-1, -1]로 나타낸다.

각 query에 대한 답을 배열에 담아, 모든 queries에 대한 답을 이차원 배열로 반환하라.

- 1 <= s.length <= 10,000
- 1 <= queries.length <= 100,000
- 0 <= firsti, secondi <= 1,000,000,000

# 문제접근

1. 브루트포스로 문제를 접근해봤다. O(query.length \* (s.length)^2) 타임 아웃이 나왔다.

- 시작이 0인 경우는 0에 대해서만 조회하고 다음으로 넘어가야 한다.

  - 0 => valid
  - 01 => not valid

- s.length의 길이가 1 ~ 10,000이다. 이를 정수형으로 나타내면 2^10,000까지 될 수 있다. 이 예외사항을 발견하지 못했었다. query가 int[]이므로 시작에서 31자리까지만 탐색하도록 제한하면 된다.

2. query가 사실상 단 하나의 값이 있는지를 확인하는 것임을 발견했다. 예를 들어 [4, 5] query는 사실상 1을 찾는 query이다. a,b가 상수일 때, `a ^ x = b일 때 x = a ^ b` 이다. 따라서 s의 부분문자열로 만들 수 있는 모든 정수와 대응하는 부분문자열의 색인을 해시 테이블에 저장한 후 바로바로 조회하여 풀 수 있다.

- O(s.length^2 + query.length)인데 s.length^2가 더 크므로 O(s.length^2)

# 코드

## 브루트포스

```java
class Solution {
    public int[][] substringXorQueries(String s, int[][] queries) {
        int[][] res = new int[queries.length][2];
        for(int i = 0; i < queries.length; i += 1){
            int[] query = queries[i];
            res[i] = findAnswer(s, query[0], query[1]);
        }
        return res;
    }
    private int[] findAnswer(String s, int xorFactor, int target){
        for(int i = 0; i < s.length(); i += 1){
            for(int j = i; (j < s.length() && j < i + 31); j += 1){
                if(s.charAt(i) == '0' && i < j) break;
                int candidate = Integer.parseInt(s.substring(i, j + 1), 2);
                if((candidate ^ xorFactor) == target){
                    return new int[]{i, j};
                }
            }
        }
        return new int[]{-1, -1};
    }
}
```

## XOR 법칙과 해시테이블 활용

71 ms, 100.5 MB

```java
class Solution {
    public int[][] substringXorQueries(String s, int[][] queries) {
        int[][] res = new int[queries.length][2];
        Map<Integer, int[]> records = new HashMap<>();
        for(int i = 0; i < s.length(); i += 1){
            for(int j = i; (j < s.length() && j < i + 31); j += 1){
                if(s.charAt(i) == '0' && i < j) break;
                int candidate = Integer.parseInt(s.substring(i, j + 1), 2);
                if(!records.containsKey(candidate)){
                    records.put(candidate, new int[]{i, j});
                }
            }
        }
        for(int i = 0; i < queries.length; i += 1){
            int[] query = queries[i];
            if(records.containsKey(query[0] ^ query[1])){
                res[i] = records.get(query[0] ^ query[1]);
            } else {
                res[i] = new int[]{-1, -1};
            }
        }
        return res;
    }
}
```
