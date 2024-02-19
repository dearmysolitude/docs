---
excerpt: 힙소트와 계산기
tags:
  - Heap
  - Sort
  - 힙
  - 정렬
Created At: 2023-04-23
---
1 2 4 4 3 5 5 6 을 힙정렬 하는 과정을 리스트 변화를 중심으로 확인해 볼 것

```python
# 1. heap 정렬 실습: 알고리즘의 구현
from typing import MutableSequence
# 이 함수는 루트노드가 힙에 맞지 않을 경우 자식 노드를 타고 내려가며 바꾸는 함수이다.
# 모든 힙 정렬을 진행하지 않음
def heap_sort(a: MutableSequence) -> None:
    def down_heap(a: MutableSequence, left: int, right: int) -> None:

        temp = a[left] # 바꿀 후보자를 임시로 저장해 놓는다

        parent  = left # 시작점인 left를 부모 노드로 두고 시작한다
        while parent < (right + 1) // 2: #노드를 내려갈 수 있을 때까지 내려가도록 한다, 이진이므로 크기의 절반
            cl = parent * 2 + 1 # 자식 노드의 주소 계산
            cr = cl + 1
            child = cr if cr <= right and a[cr] > a[cl] else cl #교환할 child를 결정. cr이 범위 내이고 + cl과 cr중 큰 것을 선택. 아니라면 cl 선정
            if temp >= a[child]: # 만약에 정렬할 필요 없다면, 즉 parent와 child를 바꿀 필요가 없다면 반복을 끝낸다
                break
            a[parent] = a[child] # 자식과 부모의 값을 바꾼다
            parent = child # 이제 자식이 부모다(부모로 바꾸어 계속 진행한다. while에 걸릴 때 까지)
        a[parent] = temp 

        n = len(a)

        for i in range((n-1)// 2, -1, -1):
            down_heap(a, i, n-1)
        
        for i in range(n-1,0, -1):
            a[0], a[i] = a[i], a[0]
            down_heap(a, 0, i-1)
            
if __name__ == '__main__':
    print('힙 정렬 수행')
    num = int(input('원소 수를 입력하세요: '))
    x = [None] * num

    for i in range(num):
        x[i] = int(input(f'x[{i}]: '))

    heap_sort(x)

    print('오름차순으로 정렬했습니다.')
    for i in range(num):
        print(f'x[{i}] = {x[i]}')
```

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216750&authkey=%21AKsf9-eBenCwjQg&width=960&height=788" alt="">
  <figcaption>힙소트 과정</figcaption>
</figure>

# 계산기

```python
import sys

prec = {
    '*': 3, '/': 3,
    '+': 2, '-': 2,
    '(': 1
}

def join_array(ints):
    str_list = list(map(str, ints))
    return " ".join(str_list)

class ArrayStack:
    def __init__(self):
        self.data = []
    def size(self):
        return len(self.data)
    def isEmpty(self):
        return self.size() == 0
    def push(self, item):
        self.data.append(item)
    def pop(self):
        return self.data.pop()
    def peek(self):
        return self.data[-1]
    def serialize(self):
        return join_array(self.data)
def convert_to_postfix(S):
    opStack = ArrayStack()
    answer = ''
    
    for w in S :
        if w in prec :
            if opStack.isEmpty() :
                opStack.push(w)
            else :
                if w == '(' :
                    opStack.push(w)
                else :
                    while prec.get(w) <= prec.get(opStack.peek()) :
                        answer += opStack.pop()
                        if opStack.isEmpty() : break
                    opStack.push(w)
        elif w == ')' :
            while opStack.peek() != '(' :
                answer += opStack.pop()
            opStack.pop()
        else :
            answer += w
        
    while not opStack.isEmpty() :
        answer += opStack.pop()
    
    return answer
def calculate(tokens):
    stack = ArrayStack()
    for token in tokens:
        if token == '+':
            stack.push(stack.pop()+stack.pop())
        elif token == '-':
            stack.push(-(stack.pop()-stack.pop()))
        elif token == '*':
            stack.push(stack.pop()*stack.pop())
        elif token == '/':
            rv = stack.pop()
            stack.push(stack.pop()//rv)
        else:
            stack.push(int(token))
        
    return stack.pop()
# infix 수식에서 공백 제거
infix = sys.stdin.readline().replace("\n", "").replace(" ", "")
postfix = convert_to_postfix(infix)
print(f"postfix : {postfix}")
result = calculate(postfix)
print(f"result : {result}")
```

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216751&authkey=%21AOlCWjiEuG_jXUI&width=721&height=961" alt="">
  <figcaption>infix → postfix</figcaption>
</figure>

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216752&authkey=%21AMnPDo_jFC608rE&width=721&height=961" alt="">
  <figcaption>Calculator</figcaption>
</figure>
