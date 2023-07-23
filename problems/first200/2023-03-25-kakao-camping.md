---
title: 카카오 코딩테스트, 캠핑

date: 2023-3-25

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://school.programmers.co.kr/learn/courses/30/lessons/1833#)

대각선으로 배치된 쐐기 2개를 이용해 텐트를 칠 수 있다. 텐트는 직사각형 모양으로 친다. 예를 들어 (2,2)과 (0,0) 두 쐐기로 텐트를 친다고 하면, (0,0) (0, 2) (2,0) (2, 2)를 꼭지점으로 하는 텐트를 치게된다. 

텐트의 넓이는 0보다 커야하고(대각선으로 배치된 쐐기를 골라야하고), 텐트 내부에 다른 쐐기가 존재해서는 안 된다. 텐트 경계에 쐐기가 존재하는 것은 가능하다. 

n개의 쐐기가 주어지고, 쐐기의 x,y 위치가 0이상의 정수로 주어질 때, 텐트를 칠 수 있는 모든 경우의 수를 반환하라.

- 같은 위치를 가지는 쐐기가 중복으로 주어지지 않는다.
- n의 범위는 2이상 5000이하

# 문제접근

1. 가능한 모든 텐트를 만들고, 텐트 중에 중간에 쐐기를 포함하는 경우를 빼주는 방식을 생각할 수 있다. 텐트를 모두 만드는 것은 n개 중 2개를 선택하는 경우의 수이므로 O(n^2)이다. 그리고 각 텐트에 대해서 쐐기를 포함하는지 포함하지 않는지 모든 쐐기를 조회하여 확인한다면, 가능한 텐트 개수(O(n^2)) * 쐐기 개수(O(n))이므로 O(n^3)이다. 이 방식으로도 내가 만든 테스트 케이스는 모두 통과한다. 하지만 문제 조건에서 n이 최대 5000이기 때문에 O(n^3)으로는 풀 수 없다.

2. 생각해보면 모든 쐐기를 모두 조회하여 텐트가 쐐기를 가지는지 확인하는 것은 다소 비효율적이다. 적어도 x축 혹은 y축 중 한 축을 기준으로 불필요한 조회를 하지 않을 수 있다. 예를 들어 쐐기의 x축 위치가 2라면, x축 위치가 4에서 10을 가지는 텐트는 이 쐐기를 조회할 이유가 없다. 이 관점을 바탕으로 최적화를 진행해보았다. 이렇게 하면 O(n^2 * 텐트의 x축 길이)로 풀 수 있다.

    - 쐐기를 x축을 기준으로 오름차순으로 정렬했다. 이렇게 하여 텐트를 생성하면 텐트 또한 왼쪽 아래쪽 좌표의 x축을 기준으로 정렬된다. 같은 x축 좌표를 가진다면 x축이 더 짧은 텐트가 먼저온다.
    - 쐐기의 x 좌표가 텐트의 오른쪽 x 좌표를 넘어서면 조회를 멈춘다.
    - 조회의 시작점이 되는 쐐기를 텐트 시작점에 맞춰 업데이트 한다.

3. 위의 방식으로도 여전히 시간초과가 발생했다. 하지만 위의 방식으로 접근하면서, 쐐기의 위치를 x축 기준으로 정렬을 한 후 반복문을 통해 텐트를 만들면 텐트의 왼쪽 x좌표가 작은 것부터 생성하고, x좌표가 같은 경우 x축 길이가 짧은 것부터 생성한다는 사실을 알았다. 이를 활용하여, 텐트를 생성하는 과정에서 텐트가 중간에 쐐기를 가지고 있는지 효율적으로 확인해 줄 수 있다. 앞서서 짝이 되는 쐐기(x좌표가 더 작은 쐐기)를 저장해놓고, 이 쐐기들에 대해서만 새로 생기는 텐트의 중간에 위치하는지 확인해주면 된다. 앞서서 짝이되는 쐐기의 개수를 B라고 할 때 O(n^2 * B)이다. B가 어떤 수일지 일반화할 수는 없지만 대체로 평균적으로 상수 정도를 기대할 수 있을 것으로 보인다.

4. 공간복잡도를 비교해보면 시간초과가 발생한 1,2번 접근은 모든 텐트를 저장하므로 O(n^2)의 공간복잡도를 가진다. 반면에 3번 째 접근은 기준점에 대해서 짝이되는 쐐기의 좌표만을 저장하며 기준점이 바뀔 때 새로운 배열을 사용하므로 최악의 경우 O(n)의 공간복잡도를 가지고, 일반적인 경우 훨씬 낮은 공간복잡도를 가진다. 

# 구현

1. 별도 Tent 클래스를 만들어 데이터와 로직을 결합하여 코드를 단순화 하였다.

```java
class Tent {
    public int[] leftDown;
    public int[] rightUp;
    
    public Tent(int[] x, int[] y){
        leftDown = new int[]{Math.min(x[0], y[0]), Math.min(x[1], y[1])};
        rightUp = new int[]{Math.max(x[0], y[0]), Math.max(x[1], y[1])};
    }
    
    public boolean containsPoint(int[] x){
        if(
            x[0] > leftDown[0] && 
            x[0] < rightUp[0] &&
            x[1] > leftDown[1] &&
            x[1] < rightUp[1]
        ) {
            return true;
        }
        return false;
    }
}
```

# 코드

## O(n^2 * 앞 서서 짝이되는 쐐기의 개수), 성공

```java
class Solution {
    public int solution(int n, int[][] data) {
        Arrays.sort(data, (a,b) -> a[0] - b[0]);
        int count = 0;
        for(int i = 0; i < n; i += 1){
            List<int[]> pairArr = new ArrayList(n);
            for(int j = i + 1; j < n; j += 1){
                if(data[i][0] == data[j][0] || data[i][1] == data[j][1]){
                    continue;
                }
                Tent newTent = new Tent(data[i], data[j]);
                boolean isAvailable = true;
                for(int k = 0; k < pairArr.size(); k += 1){
                    if(newTent.containsPoint(pairArr.get(k))){
                        isAvailable = false;
                        break;
                    }
                }
                if(isAvailable){
                    pairArr.add(data[j]);
                    count += 1;
                }
            }
        }
        return count;
    }
}
```

## 다소 최적화, O(n^2 * (텐트의 x축 길이)), 여전히 시간초과

```java
import java.util.*;

class Solution {
    public int solution(int n, int[][] data) {
        Arrays.sort(data, (a,b) -> a[0] - b[0]);
        List<Tent> tentArr = new ArrayList(n*n);
        for(int i = 0; i < n; i += 1){
            for(int j = i + 1; j < n; j += 1){
                if(data[i][0] == data[j][0] || data[i][1] == data[j][1]){
                    continue;
                }
                tentArr.add(new Tent(data[i], data[j]));
            }
        }
        int count = 0;
        int checkStartIdx = 0;
        for(int i = 0; i < tentArr.size(); i += 1){
            Tent tent = tentArr.get(i);
            if(i > 0 && tentArr.get(i-1).leftDown[0] != tent.leftDown[0]){
                checkStartIdx += 1;
            }
            boolean isAvailable = true;
            for(int j = checkStartIdx; j < n; j += 1){
                if(tent.rightUp[0] <= data[j][0]){
                    break;
                }
                if(tent.containsPoint(data[j])){
                    isAvailable = false;
                    break;
                }
            }
            if(isAvailable){
                count += 1;
            }
        }
        return count;
    }
}
```

## O(n^3) 코드, 시간초과
```java
class Solution {
    public int solution(int n, int[][] data) {
        List<Tent> tentArr = new ArrayList();
        for(int i = 0; i < n; i += 1){
            for(int j = i + 1; j < n; j += 1){
                if(data[i][0] == data[j][0] || data[i][1] == data[j][1]){
                    continue;
                }
                tentArr.add(new Tent(data[i], data[j]));
            }
        }
        int count = 0;
        for(Tent tent: tentArr){
            boolean isAvailable = true;
            for(int i = 0; i < n; i += 1){
                if(tent.containsPoint(data[i])){
                    isAvailable = false;
                    break;
                }
            }
            if(isAvailable){
                count += 1;
            }
        }
        return count;
    }
}
```

# 테스트 케이스(샘플)

1. [[0, 0], [1, 1], [0, 2], [2, 0]]: 3
2. [[0, 0], [1, 1], [2, 2], [3, 3]]: 3
3. [[0, 0], [1, 1], [2, 2], [3, 3], [2, 1], [3, 1]]: 7
4. [[0, 0], [1, 1], [2, 2], [3, 3], [2, 1], [3, 1], [3, 4]]: 9
5. [[0, 0], [1, 1], [2, 2], [3, 3], [2, 1], [3, 1], [3, 4], [4, 2]]: 14
6. [[0, 0], [1, 1], [2, 2], [3, 3], [2, 1], [3, 1], [3, 4], [4, 2], [-1, 2]]: 20
7. [[0, 0], [1, 1], [2, 2], [3, 3], [2, 1], [3, 1], [3, 4], [4, 2], [-1, 2], [-2, -1]]: 22
8. [[1, 1], [2, 2], [3, 3], [2, 1], [3, 1], [3, 4], [4, 2], [-1, 2], [-2, -1]]: 20
9. [[1, 1], [2, 2], [3, 3], [2, 1], [3, 1], [3, 4], [4, 2], [-1, 2], [-2, -1], [1, -3]]: 25 
10. [[1, 1], [2, 2], [3, 3], [2, 1], [3, 1], [3, 4], [4, 2], [-1, 2], [-2, -1], [1, -3], [2, -1]]: 29
11. [[1, 1], [-1, 1], [1, -1], [-1, -1]]: 2
