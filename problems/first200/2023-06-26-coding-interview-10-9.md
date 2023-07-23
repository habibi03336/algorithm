---
title: 연습문제, 정렬된 행렬 탐색

date: 2023-6-26

categories:
  - 문제풀이

tags:
  - Exercise
---

# 문제

행과 열이 오름차순으로 정렬된 M x N 행렬이 주어졌을 때 특정한 원소의 좌표를 찾는 메서드를 구현하라.

# 문제 접근

1. 대각선을 기준으로 이진 탐색을 진행한다. 찾으면 해당 원소의 좌표를 반환한다.

2. 만약 없으면 찾는 원소보다 작은 부분과 큰 부분이 갈리는 지점을 찾는다. 문제 조건 상 갈리는 지점으로 축을 그엇을 때, 2,4 사분면에는 찾고자 하는 원소가 있을 수 없다.

   (2 사분면에는 찾고자 하는 원소보다 작은 원소, 4 사분면에는 찾는 원소보다 큰 원소만이 존재한다.)

3. 1,3사분면에 대해서만 재귀적으로 함수를 호출한다.

4. 갈라지는 지점을 찾는데에 이진탐색을 적용하면 보다 빠른 성능을 기대할 수 있다. 다만, 대각선으로 배치된 좌표에 대해 이진탐색을 하는 것이라 다소 복잡하여 구현에 적용하지는 않았다.

<div style="text-align: center;">
    <img src="https://raw.githubusercontent.com/habibi03336/algorithm/master/assets/img/exercise_10_9.jpeg" alt="matrix search" width="500"/>
</div>

# 테스트 케이스

    1. 행렬 크기: 4 5
       행렬: 1 3 4 5 6
            2 5 6 7 10
            5 7 10 11 12
            9 13 15 17 19
       찾는 수: 9
       정답: [3, 0]

    2. 3 1
       2
       4
       6
       4
       [1, 0]

# 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {

    public static int[] solution(int[][] matrix, int[] leftUpper, int[] rightDown, int K){
        int[] searchResult = findSplittingDiagonalElemPos(matrix, leftUpper, rightDown, K);
        if (searchResult == null) return null;
        if (matrix[searchResult[0]][searchResult[1]] == K) return searchResult;
        int[] recursionRes = solution(matrix, new int[]{searchResult[0] + 1, leftUpper[1]}, new int[]{rightDown[0], searchResult[1]}, K);
        if (recursionRes == null){
            recursionRes = solution(matrix, new int[]{leftUpper[0], searchResult[1]+1}, new int[]{searchResult[0], rightDown[1]}, K);
        }
        return recursionRes;
    }

    public static int[] findSplittingDiagonalElemPos(int[][] matrix, int[] leftUpper, int[] rightDown, int K){
        int x = leftUpper[0];
        int y = leftUpper[1];
        while(x <= rightDown[0] && y <= rightDown[1]){
            if(matrix[x][y] == K) {
                return new int[]{x, y};
            }
            if(x+1 <= rightDown[0] && y+1 <= rightDown[1] && matrix[x][y] <= K && matrix[x+1][y+1] > K) {
                return new int[]{x, y};
            }
            if((x+1 > rightDown[0] || y+1 > rightDown[1]) && matrix[x][y] <= K){
                return new int[]{x, y};
            }
            x++;
            y++;
        }
        return null;
    }

    public static void main(String[] args) throws IOException {
        //input
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
        int[][] matrix = new int[N][M];
        for(int i = 0; i < N; i += 1){
            st = new StringTokenizer(br.readLine());
            for(int j = 0; j < M; j += 1){
                matrix[i][j] = Integer.parseInt(st.nextToken());
            }
        }
        st = new StringTokenizer(br.readLine());
        int K = Integer.parseInt(st.nextToken());

        //output
        System.out.println(Arrays.toString(solution(matrix, new int[]{0,0}, new int[]{N-1, M-1}, K)));
    }
}
```
