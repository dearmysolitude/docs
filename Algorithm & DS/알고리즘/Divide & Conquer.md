큰 문제를 작은 문제로 쪼개어 푼 후 합쳐서 전체 문제의 답을 찾는 기법
## Top-down / 하향식 접근
큰 문제에서 작은 문제로 내려가는 과정. 큰 문제를 작은 문제로 쪼개는 과정이 필연적으로 들어가게 된다.
사용자 친화적이지만, 하나의 방식으로 모든 부분 문제를 풀어야 하기 때문에 필요 이상으로 구현이나 설계가 복잡해질 수 있다. 내려갈수록 문제들의 풀이가 복잡해지고 각각의 문제마다 해결법이 상이해지는 경우 탑다운 방식은 적합하지 않다.
## Bottom-up / 상향식 접근법
작은 문제부터 시작해서 점점 더 큰 문제로 올라가는 과정을 내포하고 있다. 작은 문제를 큰 문제로 적절하게 합치는 과정이 필요하다.
Ad-hoc 접근이 가능하며, 각 분야에 대한 맞춤형 풀이를 제공하여 알고리즘 분야에서는 바텀업 방식에서만 사용할 수 있는 최적화 테크닉이 존재한다. 하지만 작은 문제를 합친 겨로가가 전체 문제의 답이라는 보장이 없다. 탑다운은 풀이법에 대한 틀을 잡고 내려가지만 바텀업은 시작 단계부터 이런 아웃라인을 잡기가 어렵기 때문에 풀어나가는 동안 시행 착오가 발생할 수 있다.
## 분할 정복 알고리즘의 조건
1. 문제를 쪼개나가는 과정에서, 큰 문제와 작은 문제의 구조가 동일하거나 최소한 비슷해야 한다. 큰 틀에서 봤을 때 큰 문제에서 적용되는 해법이 작은 문제에서 동일하게 적용된다.
2. 쪼개진 문제들 간에는 서로 상관 관계가 없어야 한다. 쪼개진 문제들이 풀이 과정에서 서로 영향을 주지 말아야 한다. → 영향을 주는 경우는 동적 프로그래밍이 다루는 부분이다.
재귀 함수는 이러한 조건을 잘 갖춘 방법중에 하나이다. 주어진 입력(큰 부분)을 작은 부분(작은 문제)로 재귀적으로 쪼개는 부분과 최소 단위의 문제로 쪼갰을 때 풀이하는 부분으로 나누어서 구현한다.

### 예 1. Merge Sort
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217754&authkey=%21AEzNhVt1g2Nwph8&width=1024&height=986" alt="">
</figure>
### 예 2. Binary Search
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217753&authkey=%21AMT4lIs5bT-UcwA&width=533&height=395" alt="">
</figure>