완전 이진 트리Complete Binary Tree. 즉 마지막 레벨을 제외한 모든 레벨이 완전히 채워져 있으며, 마지막 레벨의 모든 노드는 가능한한 왼쪽에 있다.
모든 노드에 저장된 값들은 자식 노드들의 것보다 우선순위가 크거나 같다. 이 때, 연결된 부모 - 자식 노드간의 크기 비교만 하면 된다. 힙으로 우선순위 큐를 구현할 때에는 노드에 저장된 값을 우선 순위로 본다. 루트 노드에 우선순위가 높은 데이터가 위치하게 된다.
## 최대 힙Max heap
이진 트리이면서, 루트 노드로 올라갈수록 값이 커지는 구조. 우선 순위는 값이 큰 순서이다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217761&authkey=%21AGB_L0IDSTpO390&width=590&height=287" alt="">
  <figcaption>Max Heap</figcaption>
</figure>

## 최소 힙Min heap
루트 노드로 올라갈수록 값이 작아진다. 우선순위는 값이 작은 순서.
새로운 노드가 입력될 때 우선순위가 낮다는 가정을 하고 맨 끝, 리프 끝에 저장하게 된다. 그 후, 부모 노드와 우선순위를 비교해 위치를 바꾸어 나간다. 삭제할 때에는 루트노드를 삭제 후 맨 하위의 노드를 옮겨 넣은 후 자식 노드 중 우선순위가 높은 노드들과 바꾸어 나간다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217760&authkey=%21AO-1iIWBJ48g-c8&width=557&height=268" alt="">
  <figcaption>Min Heap</figcaption>
</figure>

- 최대 힙이던 최소 힙이던 우선순위가 크면 루트 노드에 들어가게 된다.
- 힙의 구현은 연결리스트보다는 배열로 구현해야 한다.
- 인덱스는 1로 두는것이 직관적이고 다루기 용이하다.
- a\[i]에서, 관계 노드의 인덱스는 다음과 같이 계산할 수 있다.
	- 부모 노드: a\[(i-1)//2]
	- 왼쪽 자식: a\[i\*2 +1]
	- 오른쪽 자식:a\[i\*2+2]