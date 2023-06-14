---
title: LeetCode, 304. Range Sum Query 2D - Immutable

date: 2023-2-8

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://leetcode.com/problems/range-sum-query-2d-immutable/description/)

정수가 담긴 2d matrix가 주어진다. 2d matrix의 일부분을 나타내는 위치 값 2개가 주어진다. 하나는 왼쪽 상단, 하나는 오른쪽 하단을 나타낸다. 해당 부분에 속하는 정수의 합을 반환하는 메소드를 구현하라. 단, O(1)으로 동작해야한다.

# 문제접근

1.

<div style="text-align: center;">
    <img src="/assets/img/leetcode-304.jpeg" alt="leetcode-304" maxWidth="400"/>
</div>

- 이와 같은 개념으로 미리 구해둔 (0,0)에서 (i,j)까지의 총합을 이용해, O(1)으로 범위의 값을 구할 수 있다.
- 또한 미리 총합(sumMatrix)을 구할 때도 비슷한 개념을 활용해 dp로 총합을 효율적으로 구할 수 있다.

# 코드

```java
class NumMatrix {
    private int[][] matrix;
    private int[][] sumMatrix;
    public NumMatrix(int[][] matrix) {
        this.matrix = matrix;
        this.sumMatrix = new int[matrix.length][matrix[0].length];
        for(int i = 0; i < matrix.length; i += 1){
            for(int j = 0; j < matrix[0].length; j += 1){
                int res = this.matrix[i][j];
                if(i-1 >= 0) res += this.sumMatrix[i-1][j];
                if(j-1 >= 0) res += this.sumMatrix[i][j-1];
                if(i-1 >= 0 && j-1 >= 0) res -= this.sumMatrix[i-1][j-1];
                this.sumMatrix[i][j] = res;
            }
        }
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        int res = this.sumMatrix[row2][col2];
        if(row1-1 >= 0) res -= this.sumMatrix[row1 - 1][col2];
        if(col1-1 >= 0) res -= this.sumMatrix[row2][col1-1];
        if(row1-1 >= 0 && col1-1 >= 0) res += this.sumMatrix[row1-1][col1-1];
        return res;
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * int param_1 = obj.sumRegion(row1,col1,row2,col2);
 */
```
