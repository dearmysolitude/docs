### 직접 재귀와 간접 재귀

자신과 똑같은 함수를 호출하는 방식을 직접(direct) 재귀라고 한다. 간접(indirect) 재귀는 서로 다른 함수가 서로를 호출하는 구조이다.

유클리드 호제법: 두 정숫값이 주어질 때 큰 값을 작은 값으로 나누어 떨어지면 작은 값이 최대 공약수가 된다. 나누어 떨어지지 않으면 작은 값과 나머지에 대해 재귀적으로 시행하여 나누어 떨어지면 그 수가 최대공약수이다.

### 하향식 분석과 상향식 분석

재귀 호출을 여러 번 실행하는 함수를 순수한genuinly재귀라고 한다.

하향식 분석topdown analysis: 같은 함수를 여러번 호출할 수 있어 효율적이라고 할 수는 없음

상향식 분석bottom-up analysis

재귀 알고리즘의 비재귀적 표현: recur함수의

**백준** [#1914 하노이 탑](https://www.acmicpc.net/problem/1914) / [#10872 팩토리얼](https://www.acmicpc.net/problem/1914) / [#9663 N-Queen](https://www.acmicpc.net/problem/9663) / [#1074 Z](https://www.acmicpc.net/problem/1074)

# 재귀함수의 시간복잡도: Master Theorem

알고리즘 분석에서 재귀 관계식으로 표현한 알고리즘의 동작 시간을 점근적으로 계산하여 간단하게 계산하는 방법이다. Introduction to Algorithm 4.3~4.5절에 설명되어 유명함.
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217752&authkey=%21AI5_tUj2KuggfCg&width=955&height=464" alt="">
</figure>