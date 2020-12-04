---
title: "알고리즘 - 자료구조(1)"
excerpt: "Linked List 기본"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2020-12-01
---

## Linked List

![image](https://user-images.githubusercontent.com/46255148/100734630-503b0400-3413-11eb-8e11-ad83218fe5dc.png)

-   선형 배열이 **번호가 붙여진 칸에 원소들을 채워넣는 방식**이라고 한다면, 연결 리스트는 **각 원소들을 줄줄이 엮어서 관리하는 방식**이다.
-   따라서 인덱스가 존재하지 않는다.
-   연결 리스트에서는 원소들이 링크 (link) 라고 부르는 고리로 연결되어 있으므로, 가운데에서 끊어 하나를 삭제하거나, 아니면 가운데를 끊고 그 자리에 다른 원소를 (원소들을) 삽입하는 것이 선형 배열의 경우보다 쉽고 연산이 빠르다.
-   선형 배열과의 차이점
    1. **저장 위치** : 선형 배열은 미리 지정한 곳에 위치 vs 연결 리스트는 동적으로 임의대로 위치
    2. **저장 공간** : 선형 배열은 미리 지정한 배열의 크기만큼 공간을 할당 vs 연결 리스트는 링크가 메모리에 저장되어야 있어야 해서 메모리 요구량이 더 크다.
    3. **특정 원소 탐색** : 선형 배열은 몇번째에 있는지만 알면 매우 쉽기에 간편 O(1) vs 연결 리스트는 앞에서부터 하나하나 따라가서 찾기에 선형탐색과 유사하다. O(n)
    4. **특정 원소 삽입/삭제** : 선형 배열은 위치를 찾고 작업하고 이후의 인덱스를 변경해주는 작업 vs 연결 리스트에선 단순히 링크만 수정

## 구현

```python
class Node: # 리스트의 원소인 각 노드
    def __init__(self, item):
        self.data = item
        self.next = None


class LinkedList:
    def __init__(self):
        self.nodeCount = 0 # 리스트의 총 길이
        self.head = None # head node
        self.tail = None # tail node

    def getAt(self, pos):  # 특정 원소 탐색 (k번째) -> O(n)
        if pos < 1 or pos > self.nodeCount: # 탐색할 수 없는 노드의 위치일 경우
            return None
        i = 1
        curr = self.head
        while i < pos:
            curr = curr.next
            i += 1
        return curr

    def traverse(self):  # 리스트 순회
        answer = []
        if self.head == None:
            return answer
        curr = self.head
        while True:
            answer.append(curr.data)
            if curr.next == None:
                break
            curr = curr.next
        return answer

    def getLength(self):  # 길이 얻어내기
        return self.nodeCount

    def insertAt(self, pos, newNode):  # 특정 원소 삽입
        if pos < 1 or pos > self.nodeCount + 1: # 삽입할 수 없는 노드의 위치일 경우
            return False

        if pos == 1:  # O(1)
            newNode.next = self.head
            self.head = newNode
        else:
            # O(1) - 만약 tail포인터가 없었다면 가장 오래 걸리는 연산이었었다.
            if pos == self.nodeCount + 1:
                prev = self.tail
                self.tail = newNode
            else:  # O(n)
                prev = self.getAt(pos-1)
            newNode.next = prev.next
            prev.next = newNode

        self.nodeCount += 1
        return True

    def popAt(self, pos):  # 특정 원소 삭제
        if pos < 1 or pos > self.nodeCount: # 삭제할 수 없는 노드의 위치일 경우
            raise IndexError
        curr = None
        if pos == 1:  # O(1)
            curr = self.head
            if self.nodeCount == 1:
                self.head = None
                self.tail = None
            else:
                self.head = curr.next
        else:  # O(n)
            prev = self.getAt(pos-1)
            curr = prev.next
            if pos == self.nodeCount:
                prev.next = None
                self.tail = prev
            else:
                prev.next = curr.next
        self.nodeCount -= 1
        return curr.data

    def concat(self, L):  # 두 리스트 합치기
        self.tail.next = L.head
        if L.nodeCount: # 합치는 리스트가 비어있을 경우
            self.tail = L.tail
        self.nodeCount += L.nodeCount
```
