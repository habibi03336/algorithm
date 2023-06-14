---
title: 카카오 코딩테스트, 기둥과 보

date: 2023-5-29

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/60061)

기둥과 보를 격자의 각 점을 기준으로 설치하려고 한다. 기둥은 주어진 좌표를 기준으로 위쪽으로 설치를 한다. 보는 주어진 좌표를 기준으로 오른쪽으로 설치를 한다.

기둥과 보는 다음의 규칙에 따라 설치되어야 한다.
1. 기둥은 아래에 바닥 or 기둥 or 보가 있어야 설치될 수 있다.
2. 보는 양 끝 중 적어도 한 쪽 아래에 기둥이 있거나, 양쪽에 모두 보가 있는 경우에 설치 될 수 있다.

기둥과 보는 설치되거나 삭제 될 수 있다. 하지만 위의 조건이 한 곳에서라도 충족되지 않으면 설치나 삭제 명령이 무시된다.

설치 및 삭제 명령이 배열로 주어질 때, 기둥과 보가 설치된 최종 결과를 반환하라.

# 문제접근

1. 시뮬레이션 하면서, 조건을 만족하는지 확인하고 조건 만족시 명령을 실행하도록 했다.

# 구현

1. 삭제 시에 주변 기둥, 보가 유효한지 확인해주어야 한다. 이를 위해서 설치물을 일단 없애고 유효성을 확인한 후 다시 보드를 원상복구 시켜주는 방식을 활용하였다.

2. 설치를 하는 것은 그 자리가 유효한지만 확인을 해주면 된다. 반면 삭제의 경우 삭제했을 때 주변이 모두 유효한지 확인해주어야 한다. 따라서 특정 좌표의 유효성 검사 기능을 삭제 기능에서 여러 차례 호출하게 된다. 

3.  `기둥 추가`, `보 추가`를 나눠서 기능 개발을 하고, 삭제 기능도 `기둥 삭제`, `보 삭제`를 나누어 했다. 이렇게 나누니까 생각이 단순화 되고 기능 개발이 훨씬 자연스럽고 수월했다.

# 코드

```java
import java.util.List;
import java.util.ArrayList;

class Solution {
    static int DELETE = 0;
    static int PUT = 1;
    static int PILLAR = 0;
    static int FLOOR = 1;
    
    public int[][] solution(int n, int[][] build_frame) {
        boolean[][][] board = getInitializedBoard(n);
        for(int[] build : build_frame){
            int x = build[0];
            int y = build[1];
            if(build[3] == DELETE && build[2] == PILLAR){
                if(isDeletePillarValid(x, y, board)){
                    board[x][y][PILLAR] = false;
                }
            } else if(build[3] == DELETE && build[2] == FLOOR){
                if(isDeleteFloorValid(x, y, board)){
                    board[x][y][FLOOR] = false;
                }
            } else if(build[3] == PUT && build[2] == PILLAR){
                if(isPillarPositionValid(x, y, board)){
                    board[x][y][PILLAR] = true;
                }
            } else if(build[3] == PUT && build[2] == FLOOR){
                if(isFloorPositionValid(x, y, board)){
                    board[x][y][FLOOR] = true;
                }
            }
        }
        List<int[]> answer = new ArrayList<>();
        for(int i = 0; i < n+1; i += 1){
            for(int j = 0; j < n+1; j += 1){
                if(board[i][j][PILLAR]){
                    answer.add(new int[]{i, j, PILLAR});
                }
                if(board[i][j][FLOOR]){
                    answer.add(new int[]{i, j, FLOOR});
                }
            }
        }
        return answer.toArray(new int[3][answer.size()]);
    }
    
    public static boolean isDeletePillarValid(int x, int y, boolean[][][] board){
        boolean flag = true;
        board[x][y][PILLAR] = false;
        if(flag && y+1 < board.length && board[x][y+1][PILLAR] && !isPillarPositionValid(x, y+1, board)){
            flag = false;
        }
        if(flag && y+1 < board.length && board[x][y+1][FLOOR] && !isFloorPositionValid(x, y+1, board)){
            flag = false;
        }
        if(flag && y+1 < board.length && x-1 >= 0 && board[x-1][y+1][FLOOR] && !isFloorPositionValid(x-1, y+1, board)){
            flag = false;
        }
        board[x][y][PILLAR] = true;
        return flag;
    }
    
    public static boolean isDeleteFloorValid(int x, int y, boolean[][][] board){
        boolean flag = true;
        board[x][y][FLOOR] = false;
        if(flag && x-1 >= 0 && board[x-1][y][FLOOR] && !isFloorPositionValid(x-1, y, board)){
            flag = false;
        }
        if(flag && x+1 < board.length && board[x+1][y][FLOOR] && !isFloorPositionValid(x+1, y, board)){
            flag = false;
        }
        if(flag && board[x][y][PILLAR] && !isPillarPositionValid(x, y, board)){
            flag = false;
        }
        if(flag && x+1 < board.length && board[x+1][y][PILLAR] && !isPillarPositionValid(x+1, y, board)){
            flag = false;
        }
        board[x][y][FLOOR] = true;
        return flag;
    }
    
    public static boolean isFloorPositionValid(int x, int y, boolean[][][] board){
        if(board[x][y-1][PILLAR]){
            return true;
        }
        if(x+1 < board.length && board[x+1][y-1][PILLAR]){
            return true;
        }
        if(
            x-1 >= 0 && 
            x+1 < board.length && 
            board[x-1][y][FLOOR] && 
            board[x+1][y][FLOOR]
        ){
            return true;
        }
        return false;
    }
    
    public static boolean isPillarPositionValid(int x, int y, boolean[][][] board){
        if(y == 0 || board[x][y-1][PILLAR]){
            return true;
        }
        if(board[x][y][FLOOR]){
            return true;
        }
        if(x-1 >= 0 && board[x-1][y][FLOOR]){
            return true;
        }
        return false;
    }
    
    public static boolean[][][] getInitializedBoard(int n){
        boolean[][][] board = new boolean[n+1][n+1][2];
        return board;
    }
}
```
