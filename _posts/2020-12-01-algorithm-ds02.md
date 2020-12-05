---
title: "알고리즘 - 자료구조(2)"
excerpt: "Linked List - dummy node 추가 형태"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2020-12-01
---

## dummy node 추가

![image](https://user-images.githubusercontent.com/46255148/100736257-d8220d80-3415-11eb-8036-6a30759cf6fe.png)

-   앞서 구현한 Linked List는 특정 위치에 새로운 데이터 원소를 삽입하거나 그 위치에 있는 데이터 원소를 삭제하는 경우였다.
-   여기선 dummy node를 활용하여 **특정 원소의 바로 다음**을 지정하여 원소를 삽입/삭제하는 연산을 구현한다.
-   dummy node란 데이터를 담고 있지 않은, 그냥 자리만 차지하는 노드이다.
-   이를 통해 구현 내부에서의 많은 if 처리를 간단하게 할 수 있다.

## 구현

```python
class Node:
    def __init__(self, item):
        self.data = item
        self.next = None

class LinkedListByDummyNode:
    def __init__(self):
        self.nodeCount = 0
        self.head = Node(None)
        self.tail = None
        self.head.next = self.tail

    def getLength(self):  # 길이 얻어내기
        return self.nodeCount

    def traverse(self):  # 리스트 순회
        result = []
        curr = self.head
        while curr.next:
            curr = curr.next
            result.append(curr.data)
        return result

    def getAt(self, pos):  # k번째 원소 얻어내기
        if pos < 0 or pos > self.nodeCount:
            return None
        i = 0
        curr = self.head
        while i < pos:
            i += 1
            curr = curr.next
        return curr

    def insertAfter(self, prev, newNode):  # 특정 노드를 특정 노드 뒤에 삽입
        newNode.next = prev.next
        if prev.next is None:
            self.tail = newNode
        prev.next = newNode
        self.nodeCount += 1
        return True

    def insertAt(self, pos, newNode):  # 특정 노드를 특정 위치에 삽입
        if pos < 1 or pos > self.nodeCount + 1:
            return False
        if pos != 1 and pos == self.nodeCount + 1:
            prev = self.tail
        else:
            prev = self.getAt(pos - 1)
        return self.insertAfter(prev, newNode)

    def popAfter(self, prev):  # 특정 노드 뒤에 있는 노드 삭제
        if prev.next is None:
            return None

        curr = prev.next
        if curr.next is None:
            prev.next = None
            self.tail = prev

        prev.next = curr.next
        self.nodeCount -= 1
        return curr.data

    def popAt(self, pos):  # 특정 위치에 있는 노드 삭제
        if pos < 1 or pos > self.nodeCount:
            raise IndexError
        if pos == 1:
            prev = self.head
        else:
            prev = self.getAt(pos-1)
        return self.popAfter(prev)

    def concat(self, L):  # 두 리스트 합치기
        self.tail.next = L.head.next
        if L.nodeCount:
            self.tail = L.tail
        self.nodeCount += L.nodeCount

```
