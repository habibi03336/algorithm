---
title: 카카오 코딩테스트, 매칭 점수

date: 2023-5-14

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/42893)

여러 개의 html이 있다. 검색어 word가 주어질 때, 검색어와 html의 연관성을 나타내는 점수를 계산해보려 한다. 점수는 다음과 같은 방식으로 계산된다.

1. 점수에는 기본점수와 링크점수가 있다.
2. 기본점수는 html에 검색어가 등장하는 횟수이다.
3. 링크 점수는 해당 html을 링크하고 있는 html의 (기본점수 / 링크개수) 이다.

가장 높은 점수를 가지는 html의 index를 반환하라.

- 만약 점수가 같은 경우 더 작은 색인을 반환한다.
- 검색어는 영어 알파벳으로만 이루어져있다.
- 기본점수를 계산할 때 대소문자를 신경쓰지 않는다.
- 단어와 검색어가 완전히 일치하는 경우만 기본점수로 더해지며, 단어 간 구분은 알파벳을 제외한 모든 문자이다.
- 메타 태그 `<meta property="og:url" content="{URL}">`을 통해서 html의 url을 알 수 있다.
- a 태그 `<a href="{LINK}">`를 통해서 연결된 링크를 알 수 있다.


# 문제접근

1. html의 기본점수와 연결 관계만 파악하면 쉽게 문제를 풀 수 있다. 문자열을 분석해 기본점수와 연결 관계를 알아내는 것이 문제의 핵심이었다.

2. 해시테이블을 활용하여 링크에 해당하는 노드를 바로 접근할 수 있도록 했다. 링크점수를 반영해 줄 때 효율적으로 쓰일 수 있다.

# 구현

1. 단어 간 구분은 알파벳이 아닌 모든 문자로 한다고 되어있다. 정규표현식을 사용하여 이 작업을 쉽게 해줄 수 있었다.

2. meta태그와 a태그가 항상 정확히 일치하는 형식으로 주어진다고 가정했다. 그리고 String의 indexOf 메쏘드를 통해서 meta태그과 a태그의 위치를 파악하고 링크를 뽑아내었다.

3. a태그의 경우 여러개를 뽑아내야한다. 검색의 시작 인덱스를 설정해줄 수 있어서 이를 활용하여 전체 a태그를 찾아내었다.

# 코드

```java
import java.util.Map;
import java.util.HashMap;
import java.util.List;
import java.util.ArrayList;

class Node {
    int index;
    int score;
    float linkScore;
    String[] links;
    public Node(int index, int score, String[] links){
        this.index = index;
        this.score = score;
        this.links = links;
        this.linkScore = 0;
    }
}

class Solution {
    public int solution(String _word, String[] pages) {
        String word = _word.toLowerCase();
        Map<String, Node> map = new HashMap<>();
        for(int i = 0; i < pages.length; i += 1){
            String pageLowered = pages[i].toLowerCase();
            String pageUrl = getPageUrl(pageLowered);
            int score = getPageScore(pageLowered, word);
            String[] links = getPageLinks(pageLowered);
            map.put(pageUrl, new Node(i, score, links));
        }
        for(String key : map.keySet()){
            Node node = map.get(key);
            float addingLinkScore = ((float) node.score /  (float) node.links.length);
            for(String link : node.links){
                if(map.containsKey(link)){
                    map.get(link).linkScore += addingLinkScore;
                }
            }
        }
        int resIndex = -1;
        float maxScore = -1;
        for(String key : map.keySet()){
            Node node = map.get(key);
            if(
                node.score + node.linkScore > maxScore ||
                node.score + node.linkScore == maxScore && resIndex > node.index
            ){
                resIndex = node.index;
                maxScore = node.score + node.linkScore;
            }
        }
        return resIndex;
    }
    
    public static String getPageUrl(String page){
        String meta = "<meta property=\"og:url\" content=\"https://";
        int index = page.indexOf(meta);
        StringBuilder sb = new StringBuilder();
        int i = index + meta.length();
        while(true){
            sb.append(page.charAt(i));
            i += 1;
            if(page.length() <= i || page.charAt(i) == '"'){
                break;
            }
        }
        return sb.toString();
    }
    
    public static int getPageScore(String page, String word){
        String[] tokens = page.split("[^a-z]");
        int score = 0;
        for(int i = 0; i < tokens.length; i += 1){
            if(tokens[i].equals(word)){
                score += 1;
            }
        }
        return score;
    }
    
    public static String[] getPageLinks(String page){
        String aTag = "<a href=\"https://";
        List<String> links = new ArrayList<>();
        int findIndex = page.indexOf(aTag, 0);
        while(findIndex != -1){
            StringBuilder sb = new StringBuilder();
            int i = findIndex + aTag.length();
            while(true){
                sb.append(page.charAt(i));
                i += 1;
                if(page.length() <= i || page.charAt(i) == '"'){
                    break;
                }
            }
            links.add(sb.toString());
            findIndex = page.indexOf(aTag, i);
        }
        return links.toArray(new String[links.size()]);
    }
}
```
