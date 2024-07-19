다음 데이터를 아래의 퀵소트 알고리즘을 통해 정렬시킬 때, 데이터의 상태를 그려보시오.

→ `[3, 1, 2, 5, 3, 1]`
```python
from typing import MutableSequence

def qsort(a: MutableSequence, left: int, right: int) -> None:
		pl = left
		pr = right
		x = a[(left + right) // 2]

		while pl <= pr:
				while a[pl] < x: pl += 1
				while a[pr] > x: pr -= 1
				if pl <= pr:
						a[pl], a[pr] = a[pr], a[pl]
						pl += 1
						pr -= 1

		if left < pr: qsort(a, left, pr)
		if right > pl: qsort(a, pl, right)

def quick_sort(a: MutableSequence) -> None:
		qsort(a, 0, len(a)-1)

if __name__ == '__main__':
		print('퀵 정렬을 수행합니다.')
		num = int(input('원소 수를 입력하세요')
		x = [None]*num

		for i in range(num):
				x[i] = int(input(f'x[{i}]: ')

		quick_sort(x)

		print('오름차순으로 정렬했습니다.')
		for i in range(num):
				print(f'x[{i}] = {x[i]}')
```
