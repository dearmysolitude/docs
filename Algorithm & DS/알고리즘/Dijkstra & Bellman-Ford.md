  

## Single-Source Shortest Path Problem
- One recurring problem in graph
- Happens in
    path finding
    routing on comm. network
    social networks
- We know whrere we are
- We want to know how long to travel to our destination
Terminology: Source = whrere we start / Destination = where we arrive
- Dynamic programming에 속한다
## 다익스트라 알고리즘Dijsktra Algorithm
음의 간선이 존재하지 않는 그래프에서 사용되며, 0 이상의 양수일 경우 정상적으로 작동한다 → 음의 경로가 포함되어 있는 경우에는 벨만-포드 알고리즘을 사용한다.
문제: [1916 최소비용 구하기/백준](https://www.notion.so/230424-2573-1916-a97f432ef0be462d906280209468bd54?pvs=21) [1238 파티/백준](https://www.notion.so/Class-4-7e57d12ad2e8423f811a93c1ecec054a?pvs=21) [13549 숨바꼭질 3/백준](https://www.acmicpc.net/problem/13549)

### Pseudo Code
자세한 알고리즘에 대한 설명은 Introduction to Algorithms의 674 페이지에 잘 나와있다.
```python

V = the set of verteces
W = the set of weights on edges
s = the source vertex
  
Dijkstra’s algorithm(V, W, s)
dist = {}
  
For itr in V
        dist[v] = 99999
        dist[s] = 0
        While size(V) ≠ 0
                u = getVertexWithMinDistance(V, dist)
                V.remove(u)
                For neighbor in getNeighbors(u)
                        If dist[neighbor]>dist[u] + w(u, neighbor)
                        dist[neighbor] = dist[u] + w(u, neighbor)

return dist

```

  

### Time complexity

  

Simple implementation: $O(|E|+|V|^2) = O(|V|^2)$

  

Can be reduces to further: Binary heap in searching the next node to expand. In average case: $O((|E|+|V|)log|V|)$

  

다익스트라 알고리즘의

  

### 다익스트라 알고리즘 기본 구현

  

```python

n,m=map(int,input().split())

start = int(input())

graph = [[] for _ in range(n+1)]

  

visited=[False]*(n+1)

distance=[int(1e9)]*(n+1)

  

for _ in range(m):

    a,b,c = map(int, input().split())

    # a번 노드에서 b번 노드로가는 비용이 c인 입력

    graph[a].append((b,c))

    graph[b].append((a,c))  # 양방향일경우 반대방향도 저장 주의

  

def get_smallest_node():

    min_value=int(1e9)

    index=0

    for i in range(1,n+1):

        if distance[i]<min_value and not visited[i]:

            min_value = distance[i]

            index=i

    return index

  

def dijkstra(start):

    # 출발 기록

    distance[start]=0

    visited[start]=True

    for j in graph[start]:

        distance[j[0]]=j[1]

    # start 제외한 노드 수 만큼 반복

    for i in range(n-1):

        # 최단 거리 노드 추출

        now=get_smallest_node()

        # 방문처리

        visited[now]=True

        for j in graph[now]:

            # 현재 노드에서 갈 수 있는 다른 노드 cost 계산

            cost = distance[now]+j[1]

            # 기존 것 보다 더 짧은 경우 갱신

            if cost < distance[j[0]]:

                distance[j[0]]=cost

dijkstra(start)

for i in range(1,n+1):

    if distance[i]==int(1e9):

        print("INFINITY")

    else:

        print(distance[i])

```

  

## Bellman-Ford Algorithm

  

백준 문제: [1865 웜홀](https://www.notion.so/Class-4-7e57d12ad2e8423f811a93c1ecec054a?pvs=21)

  

다익스트라 알고리즘과 달리 O(VE)로 시간 복잡도에서 손해를 보지만, 음의 가중치가 존재하는 경우에 사용할 수 있는 알고리즘이다. 특징으로는 음의 사이클이 존재하는 경우 제대로 된 최단거리를 낼 수 없으므로 이를 판단하는 코드가 있다: 노드의 수 만큼 진행 후에도 가중치의 변동이 업데이트 된 경우 음의 사이클이 있다고 판단하는 것이다.