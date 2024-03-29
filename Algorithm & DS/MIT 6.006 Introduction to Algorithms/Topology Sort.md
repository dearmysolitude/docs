순서가 정해져있는 작업을 차례로 수행해야 할 때 순서를 결정하여 주는 알고리즘. 이전 단계를 조건으로 다음 단계가 진행되어야 하는 경우 작업 처리 순서를 결정하는 데 사용된다.

DAG(Directed Acyclic Graph)에서만 사용 가능하다. 사이클이 발생하는 경우 사용할 수 없음.

위상 정렬 알고리즘을 통해 두가지 결과를 낼 수 있다:

1. 현재 그래프가 위상 정렬이 가능한지
2. 가능하다면 위상정렬의 결과는 무엇인지

큐와 스택을 통해 구현할 수 있으며, 큐를 사용하는 것을 살펴본다.

알고리즘은 다음과 같은 과정을 가지고 있다:

1. 진입차수가 0인 정점을 큐에 삽입한다.
2. 큐에서 원소를 꺼내 연결된 모든 간선을 제거한다.
3. 간선 제거 이후에 진입차수가 0이 된 정점을 큐에 삽입한다.
4. 큐가 빌 때까지 2~3번 과정을 반복한다.

모든 원소를 방문하기 전에 큐가 빈다면 사이클이 있어 위상정렬을 할 수 없다. 모든 원소를 방문했다면 큐에서 꺼낸 순서가 위상 정렬의 결과이다.

```cpp
#include<iostream>
#include<vector>
#include<queue>
#define MAX 10

using namespace std;

int n, inDegree[MAX];
vector<int> a[MAX];
void topologySort() {
	int result[MAX];
	queue<int> q;
// 진입차수가 0인 노드를 큐에 삽입
	for(int i = 1; i <= n; i++) {
		if(inDegree[i] == 0) q.push(i);
	}
// 정렬이 수행되려면 총 n개의 노드를 방문한다
	for(int = 1; i <= n; i++) {
	//n개를 방문하기 전에 큐가 빈다면 cycle이 발생한 것
		if(q.empty() {
			printf("사이클이 발생하였음");
			return;
	}
	int x = q.front();
	q.pop();
	result[i] = x;
	for(int i = 0; i < a[x].size(); i++) {
		int y = a[x][i];
		if (--inDegree[y]==0) {
			q.push(y);
		}
	}
	for (int i = 1; i<= n;i ++) {
		printf("%d", result[i]);
	}
}
```

```python
# 위상 정렬 알고리즘
from collections import deque
import sys
input = sys.stdin.readline

v, e = map(int, input().split()) # vertices / node
indegree = [0]*(v+1) # 노드의 연결된 진입 차수
graph = [[] for i in range(v+1)] # 

# 방향 그래프에서 모든 간선 정보 받아오기
for _ in range(e):
    a, b = map(int, input().split())
    graph[a].append(b) # 정점 A에서 B로 이동 가능
    indegree[b] += 1

def topologySort():
    result = [] # 큐에서 꺼낸 결과를 입력하는 리스트
    q = deque() # 진입 차수가 0인 노드를 가져오는 큐

    for i in range(1, v+1):
        if indegree[i] == 0:
            q.append(i)
    
    while q:
        #큐에서 꺼내서 결과에 붙여 넣기
        now = q.popleft()
        result.append(now)
        #해당 원소와 연결된 노드들의 진입차수 -1
        for i in graph[now]:
            indegree[i] -= 1
            #새롭게 진입차수가 0이 되는 노드를 큐에 삽입
            if indegree[i] == 0:
                q.append(i)
    #결과 출력
    for i in result:
        print(i, end = '')

topologySort()
```

## 시간 복잡도

$O(V+E)$: 정점의 갯수 + 간선의 갯수만큼 소요되므로 매우 빠른 알고리즘 중 하나이다.