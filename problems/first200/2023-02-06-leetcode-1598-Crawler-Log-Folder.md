---
title: LeetCode, 1598. Crawler Log Folder

date: 2023-2-6

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/crawler-log-folder/description/)

파일 시스템에서의 명령어를 순차적으로 담은 문자열 배열 logs가 주어진다. 최상위 main 폴더에서 logs대로 명령어를 실행했을 때, 마지막에 main 폴더로 돌아오기 위해 필요한 최소 명령어 수를 반환하라.

명령어는 3가지가 있다.

1. 부모 폴더로 이동한다: ../
2. 현재 폴더에 머무른다: ./
3. x폴더로 이동한다: x/

- 최상위 폴더에서 ../ 명렁어를 사용하면 최상위 폴더에 머무른다.
- 이동 명령의 폴더는 항상 존재한다.

# 문제접근

1. 마지막 폴더의 깊이를 물어보는 문제이다. 명령어의 3가지 경우에 맞게 깊이를 트래킹해줄 수 있다.

# 코드

```java
class Solution {
    public int minOperations(String[] logs) {
        int depth = 0;
        for(int i = 0; i < logs.length; i += 1){
            if(logs[i].equals("../")){
                depth = Math.max(0, depth - 1);
                continue;
            }
            if(logs[i].equals("./")){
                continue;
            }
            depth += 1;
        }
        return depth;
    }
}
```
