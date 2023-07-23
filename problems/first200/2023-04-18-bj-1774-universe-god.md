---
title: 백준, 우주신

date: 2023-4-18

categories:
  - 문제풀이

tags:
  - Exercise
---

# [문제요약](https://www.acmicpc.net/problem/1774)

총 n개의 노드가 있다. 각 노드의 2차원 위치 값이 주어진다. 노드 간 거리만큼 노드를 연결하는데 비용이 든다. 

노드의 연결상태가 주어질 때, 모든 노드가 연결되기 위해서 필요한 최소 비용을 구하여라.

- n은 1이상 1,000이하

# 문제접근

1. 연결된 노드의 뭉치를 클라우드라고 할 때, 클라우드 단위로 최소 신장 트리를 하는 것과 같은 문제이다. 프림 알고리즘 방식으로 문제를 풀었다.
    
    - [최소 신장 트리 관련 정리글](https://gmlwjd9405.github.io/2018/08/28/algorithm-mst.html)

2. 기존에 연결되어 있는 노드를 하나의 클라우드로 넣어주고, 연결이 없는 노드도 클라우드로 추상화하는 것, 그리고 클라우드 간 최소 간선을 계산하기 위해 각 노드 별 거리를 모두 고려하는 것만 좀 신경 써주면 된다. 

3. 클라우드가 연결될 때마다 새로 연결된 클라우드와 아직 연결되지 않은 클라우드의 거리를 계산한다. 이것을 모든 클라우드가 연결될 때까지 하므로 클라우드의 개수를 S라고 하면 O(S^2)의 시간복잡도를 가진다. 또한 클라우드 간 거리를 계산하는 것의 시간복잡도를 (클라우드가 포함한 평균 노드 개수)^2이라고 한다면 O((n/S)^2)이다. 이를 합치면 O(S^2 * (n/S)^2)이므로, O(n^2)이라고 할 수 있다. 예시로 생각해봐도 각 노드는 다른 노드와 딱 한 번씩만 계산이 되므로 O(n^2)인 것을 확인할 수 있다.

# 코드

```python
import math
import sys
import heapq
input = sys.stdin.readline

N, M = map(int, input().split())
nodes_pos = [None for i in range(N)]
for i in range(N):
    nodes_pos[i] = tuple(map(int, input().split()))


class Cloud:
    def __init__(self):
        self.nodes = set()
        self.merged = False

    def has(self, node):
        return node in self.nodes

    def add(self, node):
        self.nodes.add(node)

    def add_nodes(self, nodes_set):
        self.nodes = self.nodes.union(nodes_set)


clouds = []
in_clouds = [False for i in range(N)]
for i in range(M):
    a, b = map(int, input().split())
    a, b = a-1, b-1
    in_clouds[a] = True
    in_clouds[b] = True
    new_cloud = None
    for cloud in clouds:
        if cloud.has(a) or cloud.has(b):
            new_cloud = cloud
            break
    if new_cloud is None:
        new_cloud = Cloud()
        new_cloud.add(a)
        new_cloud.add(b)
        clouds.append(new_cloud)
    else:
        new_cloud.add(a)
        new_cloud.add(b)

for i in range(N):
    if not in_clouds[i]:
        cloud = Cloud()
        cloud.add(i)
        clouds.append(cloud)


def calculate_min_cloud_distance(cloud1, cloud2):
    min_dist = sys.maxsize
    for node1 in cloud1.nodes:
        for node2 in cloud2.nodes:
            x1, y1 = nodes_pos[node1]
            x2, y2 = nodes_pos[node2]
            dist = math.sqrt((x2 - x1) ** 2 + (y2 - y1) ** 2)
            min_dist = min(min_dist, dist)
    return min_dist


pq = []
clouds[0].merged = True
for i, cloud in enumerate(clouds):
    if not cloud.merged:
        dist = calculate_min_cloud_distance(clouds[0], cloud)
        heapq.heappush(pq, (dist, i))


dist_total = 0
while True:
    while clouds[pq[0][1]].merged:
        heapq.heappop(pq)
    smallest_dist, i = heapq.heappop(pq)
    merge_cloud = clouds[i]
    merge_cloud.merged = True
    dist_total += smallest_dist
    for i, cloud in enumerate(clouds):
        if not cloud.merged:
            dist = calculate_min_cloud_distance(merge_cloud, cloud)
            heapq.heappush(pq, (dist, i))

    if sum([c.merged for c in clouds]) == len(clouds):
        break

print("{:.2f}".format(dist_total))

```
