---
title: 카카오 코딩테스트, 불량사용자

date: 2023-3-30

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/64064#)

사용자의 아이디가 담긴 배열 A가 주어진다. 또한 사용자의 아이디의 일부를 "*"로 마스킹한 담긴 배열 B가 주어진다. A의 아이디 중 몇 개가 마스킹된 채 B에 있는 상황이다. B의 마스킹된 아이디를 A의 원래 아이디로 바꾼다고 할 때, 총 몇가지 경우가 가능한지 반환하라.

- A에 중복된 아이디는 없다. B에도 A의 아이디가 중복되어 들어가는 경우는 없다.
- A 배열의 길이는 1이상 8이하이다.
- 아이디 문자열의 길이 또한 1이상 8이하이다.
- 경우의 수를 셀 때 순서는 상관하지 않는다.

# 문제접근

1. 문제 조건이 매우 제한적이라서 브루트포스로 제한 할 수 있다고 판단했다. dfs와 백트래킹 방식으로 모든 경우를 탐색하고, 정렬과 set객체를 활용하여 중복을 제거하는 방식으로 풀었다.

# 코드
68.56ms, 117MB
```java
import java.util.*;

class Solution {
    private List<String> userBannedIds;
    private Set<String> caseSet;
    
    public int solution(String[] user_id, String[] banned_id) {
        userBannedIds = new ArrayList();
        caseSet = new HashSet();
        generateCaseSet(user_id, banned_id, 0);
        return caseSet.size();
    }
    
    private void generateCaseSet(String[] user_id, String[] banned_id, int banned_idx){
        
        if(banned_idx == banned_id.length){
            // userBannedIds가 backtracking이 사용하는 배열이므로, 부수효과가 없게 sorting을 해야한다.
            // 따라서 복사를 하여 새로운 객체를 만들어 정렬을 한다.
            List<String> sorted = new ArrayList<String>(userBannedIds);
            sorted.sort(Comparator.naturalOrder());
            String key = String.join(",", sorted);
            caseSet.add(key);
            return;
        }
        String bannedId = banned_id[banned_idx];
        for(int i = 0; i < user_id.length; i += 1){
            if(user_id[i] == null){
                continue;
            }
            if(isPossibleBannedId(user_id[i], bannedId)){
                String tmp = user_id[i];
                // user_id가 선택됐음을 나타내기 위해 null을 넣어놓는다.
                user_id[i] = null;
                userBannedIds.add(String.valueOf(i));
                generateCaseSet(user_id, banned_id, banned_idx+1);
                // 백트래킹이 끝나면 원래의 상태로 돌려놓는다.
                userBannedIds.remove(userBannedIds.size()-1);
                user_id[i] = tmp;
            }
        }
    }
    
    private boolean isPossibleBannedId(String userId, String bannedId){
        if(userId.length() != bannedId.length()){
            return false;
        }
        for(int j = 0; j < bannedId.length(); j += 1){
            if(bannedId.charAt(j) == '*'){
                continue;
            }
            if(userId.charAt(j) != bannedId.charAt(j)){
                return false;
            }
        }
        return true;
    }
}
```

# 비트마스크를 활용한 풀이
[비트마스크를 활용하는 풀이도 발견했다.](https://school.programmers.co.kr/learn/courses/30/lessons/64064/solution_groups?language=java) 풀이는 더 깔끔했지만 성능은 최대 4배 정도 더 안 좋았고, 메모리 성능도 비슷하거나 더 안좋았다.
316.43ms, 391MB
```java
import java.util.HashSet;
import java.util.Set;

public class Solution {

    Set<Integer> set;

    public int solution(String[] user_id, String[] banned_id) {
        set = new HashSet<>();

        go(0, user_id, banned_id, 0);

        return set.size();
    }

    public void go(int index, String[] user_id, String[] banned_id, int bit) {

        if(index == banned_id.length) {
            set.add(bit);
            return;
        }

        String reg = banned_id[index].replace("*", "[\\w\\d]");
        for(int i=0; i<user_id.length; ++i) {
            if((((bit>>i) & 1) == 1) || !user_id[i].matches(reg)) continue;
            go(index + 1, user_id, banned_id, (bit | 1<<i));
        }

    }

}
```