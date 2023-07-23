---
title: LeetCode, 433. Minimum Genetic Mutation

date: 2023-2-15

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/minimum-genetic-mutation/description/)

문자 'A', 'C', 'G', 'T'로 이루어진 길이가 8인 문자열인 유전자가 주어진다. 유전자의 한 문자가 'A', 'C', 'G', 'T' 중 다른 문자로 바뀌는 것을 mutation이라고 한다. startGene 유전자가 endGene으로 바뀌기 위해 필요한 최소 변이 개수를 구하여라. 다만 유효한 유전자 배열을 담은 bank가 주어진다. bank에 존재하는 유전자만 거쳐서 변이할 수 있다.

- 0 <= bank.length <= 10

# 문제접근

1. 이 문제는 경로 찾기 문제로 치환하여 접근할 수 있다. startGene 노드에서 출발해 endGene으로 가는 문제이다. startGene 노드는 bank에서 하나의 문자만 다른 유전자 노드와 연결되어 있다고 할 수 있다.

- 최소 경로를 묻기 때문에 이미 앞선 노드와 연결된 노드는, 그 뒤에 있는 노드와 연결 가능하더라도 연결 가능하지 않은 것으로 본다. 최소 경로를 구하는데 이미 접근 가능한 노드를 돌아돌아 갈 이유가 없다.

2. 변이 가능성 여부를 물었다면 dfs로 풀 수 있었겠지만, 최소 경로를 물었으므로 bfs로 푸는 것이 보다 간단하고 효율적이다.

# 코드

```java
class Solution {
    public int minMutation(String startGene, String endGene, String[] bank) {
        Queue<String> que = new LinkedList<>();
        int count = 0;
        que.add(startGene);
        while(que.size() != 0){
            int queSize = que.size();
            for(int i = 0; i < queSize; i += 1){
                String gene = que.poll();
                if(gene.equals(endGene)) return count;
                for(int j = 0; j < bank.length; j += 1){
                    String bankGene = bank[j];
                    if(bankGene == null) continue;
                    int charDiffCount = 0;
                    for(int k = 0; k < 8; k += 1){
                        if(gene.charAt(k) != bankGene.charAt(k)){
                            charDiffCount += 1;
                        }
                    }
                    if(charDiffCount == 1){
                        bank[j] = null;
                        que.add(bankGene);
                    }
                }
            }
            count += 1;
        }
        return -1;
    }
}
```
