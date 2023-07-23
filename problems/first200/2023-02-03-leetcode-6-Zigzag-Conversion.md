---
title: LeetCode, 6. Zigzag Conversion

date: 2023-2-3

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/zigzag-conversion/description/)

문자열 s와 자연수 numRows가 주어진다. 문자열 s를 numRows개의 줄에 걸쳐서 지그재그로 적으려고 한다. 지그재그로 적었을 때 위에서부터 같은 줄에 있는 문자를 순서대로 합친 결과를 반환하라.

# 문제접근

1. 지그재그로 적히는 것을 시뮬레이션 하여 ArrayList의 array에 문자를 넣어주고, 이를 합쳐서 반환한다.

# 엣지케이스

1. 첫 번째 줄로 돌아오기 직전까지를 하나의 슬롯으로 보고 끊어서 생각해줬다. 이 때 하나의 슬롯은 numRows \* 2 - 2의 크기들 가진다. 단 numRows가 1일 때만 0이 아닌 1의 값을 가진다. 이를 예외처리 해주어야 했다.

# 코드

```java
class Solution {
    public String convert(String s, int numRows) {
        if(numRows == 1) return s;
        ArrayList<Character>[] board = new ArrayList[numRows];
        for (int i = 0; i < numRows; i++) {
            board[i] = new ArrayList<Character>();
        }
        int slotSize = numRows*2 - 2;
        int slotNums = (s.length() / slotSize) + 1;
        for(int i = 0; i < slotNums; i += 1){
            int j = 0;
            int offset = slotSize * i;
            while(j < numRows && offset + j < s.length()){
                board[j].add(s.charAt(offset + j));
                j += 1;
            }
            int k = 2;
            while(j < slotSize && offset + j < s.length()){
                board[numRows-k].add(s.charAt(offset + j));
                j += 1;
                k += 1;
            }
        }
        String res = "";
        for(int i = 0; i < numRows; i += 1){
            for(int j = 0; j < board[i].size(); j += 1){
                res += board[i].get(j);
            }
        }
        return res;
    }
}
```
