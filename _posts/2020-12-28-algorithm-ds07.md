---
title: "알고리즘 - 자료구조(7)"
excerpt: "Priority Queue 개념 및 구현"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2020-12-28
---

## 우선순위 큐(Priority Queue)

-   선입선출의 원리가 아닌 원소들 사이의 우선순위에 따르는 자료구조이다.
-   dequeue연산은 똑같지만, enqueue연산을 할 때엔 원소들의 우선순위에 따른 알맞은 자리에 원소가 들어간다.
-   운영체제에서 CPU 스케줄러를 구현할 때, 현재 실행할 수 있는 작업들 중 가장 우선순위가 높은 것을 골라 실행하는 알고리즘과 유사하다.
-   큐에 원소를 넣을 때 (enqueue할 때) 우선순위에 따른 알맞은 자리를 찾아 넣어서 큐를 정렬된 형태로 유지시킨다. -> O(n)
-   큐에서 원소를 꺼낼 땐 (dequeue할 때) 한 쪽 끝에서 꺼낸다. -> O(1)
-   아래 예시는 큐에 원소의 값이 작은 순서가 우선순위를 가지고 enqueue연산이 일어나는 흐름을 구현한다.

![image](https://user-images.githubusercontent.com/46255148/103147552-eacffe00-4799-11eb-8194-f88b4868af77.png)

## 구현

```python
class PriorityQueue:
    def __init__(self):
        self.data = DoublyLinkedList()

    def size(self):
        return self.data.getLength()

    def isEmpty(self):
        return self.size() == 0

    def enqueue(self, item):
        node = Node(item)
        curr = self.data.head
        while curr.next != self.data.tail and curr.next.data > item:
            curr = curr.next
        self.data.insertAfter(curr, node)

    def dequeue(self):
        return self.data.popAt(self.data.getLength())

    def peek(self):
        return self.data.getAt(self.data.getLength()).data


# DoublyLinkedList
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
        if pos < 0 or pos > self.nodeCount:
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
