---
title: "알고리즘 - 자료구조(5)"
excerpt: "Queue 개념 및 구현"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2020-12-26
---

## 큐(queue)

<img src="https://user-images.githubusercontent.com/46255148/103147152-9fb3ec00-4795-11eb-942a-81b981932709.png" height="100px" width="280px"> ->
<img src="https://user-images.githubusercontent.com/46255148/103147169-d427a800-4795-11eb-9527-38af3976cddd.png" height="100px" width="280px"> ->
<img src="https://user-images.githubusercontent.com/46255148/103147204-7fd0f800-4796-11eb-95b7-4fa6f2686eb6.png" height="100px" width="280px"> ->
<img src="https://user-images.githubusercontent.com/46255148/103147223-a000b700-4796-11eb-8927-4c8fe77aacb8.png" height="100px" width="280px">

-   선입선출 (First in First out)
-   큐도 스택, 연결리스트, 선형배열리스트와 마찬가지로 자료를 보관할 수 있는 선형 구조이다.
-   원소를 추가하는 작업을 enqueue,
-   원소를 추출하는 작업을 dequeue라고 명명한다.
-   큐의 활용 : 자료를 생성하는 작업과 그 자료를 이용하는 작업이 비동기적으로 일어나는 경우
    -   네트워크 인터페이스를 통하여 도착하는 패킷 (packet) 들이 큐에 쌓여서, 도착한 순서대로 적절한 응용 프로그램에게 데이터를 보내주는 기능을 운영체제 내에서 구현
    -   서로 다른 응용 프로그램들이 프린터에 인쇄할 문서를 보낼 때, 운영체제 내에서는 프린팅 큐라는 자료 구조를 유지하여 프린터에 보내진 인쇄할 문서들의 순서를 유지
-   from pythonds.basic.queue import Queue에서 큐 자료구조를 사용할 수 있다.
-   스택을 구현할 땐 선형배열로 구현해도 성능이 괜찮았는데, 큐를 선형배열로 구현하면 dequeue연산의 시간복잡도가 O(n)으로 안 좋아진다.
-   이를 해결하기 위해 연결리스트로 큐를 구현하면 dequeue연산이 상수시간 O(1)으로도 가능해진다.

## 구현

1.  파이썬에 내장되어 있는 Array List 자료구조인 리스트를 활용

    ```python
    class ArrayListQueue:
        def __init__(self):
            self.data = []

        def size(self):
            return len(self.data)

        def isEmpty(self):
            return len(self.data) == 0

        def enqueue(self, item):
            self.data.append(item)

        def dequeue(self):  # 이 메소드만 시간복잡도 O(n)
            return self.data.pop(0)

        def peek(self):
            return self.data[0]
    ```

2.  Doubly Linked List 자료구조 활용

    ```python
    class LinkedListQueue:
        def __init__(self):
            self.data = DoublyLinkedList()

        def size(self):
            return self.data.getLength()

        def isEmpty(self):
            return self.size() == 0

        def enqueue(self, item):
            node = Node(item)
            self.data.insertAt(self.size()+1, node)

        def dequeue(self):  # 시간복잡도 O(1)
            return self.data.popAt(1)

        def peek(self):
            return self.data.getAt(1).data


    # Doubly Linked List
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
