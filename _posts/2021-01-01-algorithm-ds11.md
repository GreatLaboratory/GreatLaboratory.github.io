---
title: "알고리즘 - 자료구조(11)"
excerpt: "Heap 개념 및 구현"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2021-01-01
---

## 힙(Heap)

![image](https://user-images.githubusercontent.com/46255148/103433575-ad0a2400-4c36-11eb-9179-b11279ffa7bd.png)

-   특별한 조건을 만족시키는 이진 트리의 한 종류이다.
    1. root 노드가 언제나 최댓값 또는 최솟값을 가진다. -> 최대 힙(max heap) 또는 최소 힙(min heap)
    2. 완전 이진 트리여야 한다.
    3. 재귀적으로 정의하면 어느 노드를 루트로 하는 서브트리도 모두 최대 또는 최소 힙이다.
-   이진 탐색 트리는 원소들이 완전히 크기 순서대로 정렬되어 있지만, 힙은 느슨한 정렬이라 정렬되어있지 아니함.
-   이진 탐색 트리는 특정 키 값을 가지는 원소를 빠르게 탐색할 수 있지만, 힙은 그렇지 못하다.
-   완전 이진 트리이기 때문에 배열로 구현하는 것이 나쁘지 않다.
-   최대 힙에 원소를 삽입, 삭제하는 연산의 시간 복잡도의 최악이 O(logn)으로 빠르다. 큰 장점이다.

## 구현

```python
class MaxHeap:
    def __init__(self):
        self.data = [None]

    def insert(self, item):
        self.data.append(item)
        if len(self.data) != 2:  # 비어있는 힙이면 원소 append하고 끝
            i = len(self.data) - 1  # 삽입한 원소의 인덱스
            j = i // 2  # 삽입한 원소의 부모 인덱스

            while self.data[i] > self.data[j]:
                self.data[i], self.data[j] = self.data[j], self.data[i]
                i = j
                j //= 2
                if j == 0:  # 삽입하려는 원소가 root노드가 되면 종료
                    break

    def remove(self):  # 최대 노드 삭제 연산만 가능
        if len(self.data) == 1:
            data = None
        else:
            self.data[1], self.data[-1] = self.data[-1], self.data[1]
            data = self.data.pop()
            self.maxHeapify(1)
        return data

    def maxHeapify(self, i): # 재귀 호출
        left = 2 * i
        right = 2 * i + 1
        smallest = i
        if len(self.data) > left and self.data[left] > self.data[smallest]:
            smallest = left

        if len(self.data) > right and self.data[right] > self.data[smallest]:
            smallest = right

        if smallest != i:
            self.data[smallest], self.data[i] = self.data[i], self.data[smallest]
            self.maxHeapify(smallest)
```

## 최대/최소 힙의 응용

1. 우선 순위 큐
    - 이전엔 양방향 연결리스트로 구현해서 enqueue는 O(n), dequeue는 O(1)의 시간 복잡도를 가졌지만
    - 최대 힙으로 구현한다면 enqueue할 때 느슨한 정렬을 이루도록 넣어서 O(logn)의 시간 복잡도를 가지고
    - dequeue할 때는 최댓값을 순서대로 추출하여 똑같이 O(logn)의 시간 복잡도를 가져서 양방향 연결리스트로 구현하는 것보다 시간효율성이 좋아진다.
2. 힙 정렬
    1. 정렬되지 않은 원소들을 아무 순서대로 최대 힙에 삽입. -> O(logn)
    2. 힙이 비게 될 때까지 하나씩 최대 원소 삭제 -> O(logn)
    3. 원소들이 삭제되는 순서가 원소들의 정렬 순서가 된다. 최종 정렬 알고리즘의 복잡도 -> O(nlogn)

## 힙 정렬 구현

```python
def heapSort(unsorted):
    H = MaxHeap()
    for item in unsorted:
        H.insert(item)
    sorted = []
    d = H.remove()
    while d:
        sorted.append(d)
        d = H.remove()
    return sorted
```
