---
title: LeetCode, 819. Most Common Word

date: 2023-2-13

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/most-common-word/description/)

문장이 담겨있는 문자열 paragraph가 주어진다. 또 제외해야할 단어의 배열인 banned가 주어진다. paragraph에 있는 단어 중 banned에 있는 단어를 제외하고 가장 많이 쓰인 단어를 반환하라.

- 1 <= paragraph.length <= 1000
- paragraph는 영어 알파벳, " ", "!?',;."로 이루어져있다.
- 대소문자를 구분하지 않는다.
- banned\[i\]는 영어 소문자로만 이루어져있다.

# 문제접근

1. 단어의 개수를 해시 테이블을 통해서 세어주면 충분히 풀 수 있는 문제라고 생각했다. 다만 paragraph를 단어로 가공하는데 신경을 써줘야한다. String 객체의 replaceAll 그리고 split 메쏘드와 정규표현식을 활용하여 가공해주었다.

2. paragraph를 탐색하는데 O(p.length), 단어를 카운팅 하는데 O(단어개수), banned를 제외하는데 O(banned.length), 제일 많이 나온 단어를 조회하는데 O(단어개수)이다. 따라서 전체 O(p.length)로 풀 수 있다.

# 코드

```java
class Solution {
    public String mostCommonWord(String paragraph, String[] banned) {
        paragraph = paragraph.replaceAll("[!?',;.]", " ");
        String[] words = paragraph.split("[\" \"]{1,}");
        Map<String, Integer> counter = new HashMap<>();
        for(int i = 0; i < words.length; i += 1){
            String w = words[i].toLowerCase();
            if(counter.containsKey(w)){
                counter.put(w,  counter.get(w)+1);
            } else {
                counter.put(w, 1);
            }
        }
        for(int i = 0; i < banned.length; i += 1){
            if(counter.containsKey(banned[i])){
                counter.put(banned[i], -1);
            }
        }
        int maxCount = 0;
        String resWord = null;
        for (String k : counter.keySet()) {
            if(counter.get(k) > maxCount){
                maxCount = counter.get(k);
                resWord = k;
            }
        }
        return resWord;
    }
}
```
