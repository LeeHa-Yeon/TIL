# Heap

- 최댓값 및 최솟값을 찾아내는 연산을 빠르게 하기 위해 고안된 완전이진트리(complete binary tree)를 기본으로 한 자료구조(tree-based structure)이다.

- 키값의 대소관계는 오로지 부모노드와 자식노드 간에만 성립하며, 특히 형제 사이에는 대소관계가 정해지지 않는다

- 항상 자식은 두개밖에 없고, leaf의 가장 왼쪽부터 채움

- logn의 속도

## 힙 종류

1. 부모노드의 키값이 자식노드의 키값보다 항상 큰 힙을 '최대 힙'
2. 부모노드의 키값이 자식노드의 키값보다 항상 작은 힙을 '최소 힙'

## 힙 파이썬 라이브러리

### 1. **기존 리스트를 힙으로 변환**

이미 원소가 들어있는 리스트 힙으로 만들려면 `heapq` 모듈의 `heapify()`라는 함수에 사용하면 됩니다.

```python
heap = [4, 1, 7, 3, 8, 5]
heapq.heapify(heap)
print(heap) # [1, 3, 5, 4, 8, 7]
```

### 2. **힙에 원소 추가**

`heapq` 모듈의 `heappush()` 함수를 이용하여 힙에 원소를 추가할 수 있습니다. 첫번째 인자는 원소를 추가할 대상 리스트이며 두번째 인자는 추가할 원소를 넘깁니다

```python
heapq.heappush(heap, 4)
heapq.heappush(heap, 1)
heapq.heappush(heap, 7)
heapq.heappush(heap, 3)
print(heap)  # [1, 3, 7, 4]
```

### 3.**힙에서 원소 삭제**

`heapq` 모듈의 `heappop()` 함수를 이용하여 힙에서 원소를 삭제할 수 있습니다. 원소를 삭제할 대상 리스트를 인자로 넘기면, 가장 작은 원소를 삭제 후에 그 값을 리턴합니다.

```python
print(heapq.heappop(heap)) # 1
print(heap) # [3, 4, 7]
```

### 4. **[응용] 최대 힙**

`heapq` 모듈은 최소 힙(min heap)을 기능만을 동작하기 때문에 최대 힙(max heap)으로 활용하려면 약간의 요령이 필요합니다. 바로 힙에 튜플(tuple)를 원소로 추가하거나 삭제하면, 튜플 내에서 맨 앞에 있는 값을 기준으로 최소 힙이 구성되는 원리를 이용하는 것입니다.

```python
import heapq

nums = [4, 1, 7, 3, 8, 5]
heap = []

for num in nums:
    heapq.heappush(heap, (-num, num))  # (우선 순위, 값)

while heap:
    print(heapq.heappop(heap)[1])  # index 1
```
