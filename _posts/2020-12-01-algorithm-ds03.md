---
title: "알고리즘 - 자료구조(3)"
excerpt: "Doubly Linked List"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2020-12-01
---

## 양방향 연결 리스트(Doubly Linked List)

![image](https://user-images.githubusercontent.com/46255148/100736840-a9f0fd80-3416-11eb-9473-d915a1167c18.png)

-   지금까지의 연결 리스트에서는 링크가 한 방향으로, 즉 앞에서 뒤로 나아간다.
-   다시 말하면 먼저 오는 데이터 원소를 담은 노드로부터 그 뒤에 오는 데이터 원소를 담은 노드를 향하는 방향으로만 연결되어 있었다.
-   양방향 연결 리스트에서는 노드들이 앞/뒤로 연결되어 있다.
-   더미 노드인 head와 tail이 양 끝에 붙어있게 된다.
-   단점
    -   단방향 연결 리스트에 비해 단점은 링크를 나타내기 위한 메모리 사용량이 늘어나는 것이다.
    -   또한 원소를 삽입/삭제하는 연산에서 앞/뒤 링크를 모두 조정해 줘야해서 복잡해진다.
-   장점
    -   데이터 원소들을 차례로 방문할 때, 앞에서부터 뒤로도 할 수 있지만 뒤에서부터 앞으로도 할 수 있다.
    -   기존에 가장 마지막에 있는 원소를 삭제하고 반환할 때는 tail로 값에 접근할 수는 있지만 앞에 있는 노드의 next를 None으로 변경하는 작업을 못한다. 하지만 이젠 prev로 조절할 수 있기에 막전까지 순회하지 않아도 되서 엄청 연산이 빨라진다.

## 구현

```python
class Node:
    def __init__(self, item):
        self.data = item
        self.prev = None
        self.next = None


class DoublyLinkedList:
    def __init__(self):
        self.nodeCount = 0
        self.head = Node(None)
        self.tail = Node(None)
        self.head.prev = None
        self.head.next = self.tail
        self.tail.prev = self.head
        self.tail.text = None

    def getLength(self):  # 길이 얻어내기
        return self.nodeCount

    def traverse(self):  # 리스트 순회
        result = []
        curr = self.head
        while curr.next.next:
            curr = curr.next
            result.append(curr.data)
        return result

    def reverse(self):  # 리스트 역순회
        result = []
        curr = self.tail
        while curr.prev.prev:
            curr = curr.prev
            result.append(curr.data)
        return result

    def getAt(self, pos):
        if pos < 1 or pos > self.nodeCount:
            return None

        if pos > self.nodeCount // 2:
            i = 0
            curr = self.tail
            while i < self.nodeCount - pos + 1:
                i += 1
                curr = curr.prev
            return curr
        else:
            i = 0
            curr = self.head
            while i < pos:
                i += 1
                curr = curr.next
            return curr

    def insertAfter(self, prev, newNode):  # 특정 원소 삽입
        next = prev.next
        newNode.prev = prev
        newNode.next = next
        prev.next = newNode
        next.prev = newNode
        self.nodeCount += 1
        return True

    def insertBefore(self, next, newNode):   # 특정 원소 삽입
        prev = next.prev
        newNode.prev = prev
        newNode.next = next
        prev.next = newNode
        next.prev = newNode
        self.nodeCount += 1
        return True

    def insertAt(self, pos, newNode):  # 특정 원소 삽입
        if pos < 1 or pos > self.nodeCount+1:
            return False
        prev = self.getAt(pos-1)
        return self.insertAfter(prev, newNode)

    def popAfter(self, prev):  # 특정 원소 삭제
        curr = prev.next
        next = curr.next
        prev.next = next
        next.prev = prev
        self.nodeCount -= 1
        return curr.data

    def popBefore(self, next):  # 특정 원소 삭제
        curr = next.prev
        prev = curr.prev
        next.prev = prev
        prev.next = next
        self.nodeCount -= 1
        return curr.data

    def popAt(self, pos):  # 특정 원소 삭제
        if pos < 1 or pos > self.nodeCount:
            return None
        prev = self.getAt(pos-1)
        return self.popAfter(prev)

    def concat(self, L):  # 두 리스트 합치기
        self.tail.prev.next = L.head.next
        if L.nodeCount:
            self.tail = L.tail
        self.nodeCount += L.nodeCount
```
