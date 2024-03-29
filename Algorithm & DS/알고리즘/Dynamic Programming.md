예: Fibonacci sequence / Assembly Line scheduling

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217759&authkey=%21AK87QzkwWWKvrdg&width=1000&height=497" alt="">
  <figcaption>동적 프로그래밍</figcaption>
</figure>

A general algorithm design technique for solving problems defined by or formulated as _**recurrences with overlapping sub-instances**_.

Main storyline: Setting up a recurrence

- Relating a solution of a larger instance to solutions of some smaller instances
- Solve small instances once
- Record solutions in a table
- Extract a solution of a larger instance from the table

In this context, programming == planning

“Careful brute force”: 완전 탐색은 지수 시간이 걸리지만, DP를 통해 polynomial time이 걸리게 하는 것이 가능하다. 다항 시간이 걸리는 알고리즘 중 DP를 사용하는 알고리즘이 많다. 적용이 항상 가능하지는 않지만. Subproblems + Reuse

DP에서는 (분할 정복과 달리)분할된 부분이 중복해서 존재할 수 있다. Table(1~3차원 리스트 형태)에 저장한 부분 결과값을 필요할 때마다 불러오므로 선형의 시간 복잡도가 나타난다.

분할 정복과의 차이점: 분할 정복에서는 겹치지 않는 부분 결과를 재귀함수를 종료하며 전체 문제의 답을 도출하며, DP에서는 겹칠수 있는 문제를 테이블에 저장하여 필요할 때 선형 검색을 통해 불러오는 과정을 거쳐 답을 도출하게 된다. 이전 단계에서의 결과를 사용한다는 점에서 수학에서 사용하는 점화식의 컨셉을 반용한 것으로도 볼 수 있다.

## 피보나치 수열의 예

- subproblem으로 분해: $F(n)$은 $F(n-1)$과 $F(n-2)$의 subproblem으로 분해할 수 있다.
- $F(n) = F(n-1) + F(n-2)$으로 원래 문제를 subproblem으로 나타낸 것을 optimal structure을 가지고 있다고 한다($F(n)$이 subproblem들의 해결책을 통해 원래 문제의 해결의 실마리가 되므로).
- 이 subproblem들이 서로 overlapping하므로(), DP를 적용할 수 있다.

Divide and Conquer와 Greedy는 모두 optimal substructure을 가지고 있지만, overlapping하지 않기 때문에 DP와는 다르다.

## DP의 요소

1. Overlapping subproblems: smaller versions of the original problem that are re-used multiple times.
2. Optimal Substructure: optimal solution can be formed from optimal solutions to the overlapping subproblmes of the original problem

이러한 특성 때문에 점화식 관계를 가지는

## Memoization & Tabulation: DP algorithm

### Top-down: Memoization

Key technique of dynamic programming

Simply put: **Storting the results of previous function calls** to **reuse** the results again in the future

More philosophical sense: Bottom-up approach for problem solving

- Recursion: Top-down of divide and conquer / Stack
- Dynamic programming: Bottom-ip of storing and building / Table

피보나치 수열의 예로 설명해보자.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217758&authkey=%21ADJ5uBzfPLaRiuw&width=960&height=540" alt="">
  <figcaption>Fibonacci 수열을 그래프로 표현한 그림</figcaption>
</figure>

Memoization으로 효용성을 챙기며 recursion으로 구현된다. 피보나치 수열을 접근할 때, $F(n)$을 구하기 위해 $F(n-1)$과 $F(n-2)$에 접근하는 것을 예로 들 수 있다. 재귀적으로 이를 구하는 것은 불필요한 반복 작업을 내포하고 있다.

이 경우, 계산해야하는 함수가 늘어날수록 시간 복잡도는 지수적으로 증가한다. 이를 효율화한 것이 memoize이다.

Memoizing은 function call의 결과를 저장하는 도구로, 보통 hashmap이나 배열로 되어 있으며, 중복되는 function call을 memoized결과를 가져옴으로써 다시 연산하는 것을 줄일 수 있다.

### Bottom-up: tabulation
Base cases를 시작으로 하여 iteration으로 구현된 것이다. 다음의 피보나치 수열의 방식이다.

```python
n = int(input()) # 입력 받은 배열의 길이
F = [0 For _ in range(n+1)]
F[0] = 0
F[1] = 1
for i in range(2, n):
	F[i] = F[i-1] + F[i-2]
```

## When to use DP
어떻게 문제에 접근하는지가 중요한 점을 차지한다. 앞서 나온 overlapping subproblems와 optimal substructure의 definition으로는 알아차리기 힘든 반면, DP문제들의 다음 특성을 살펴보자.

1. optimum value를 물어보는 경우-DP 이더라도 아닐 수 있음
2. 앞으로 할 결정이 현재/과거 결정에 의해 영향을 받는 경우. 즉 이전 단계가 다음 단계에 영향을 끼치는 경우