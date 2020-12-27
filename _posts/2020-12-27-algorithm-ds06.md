---
title: "알고리즘 - 자료구조(6)"
excerpt: "Circular Queue 개념 및 구현"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2020-12-27
---

## 환형 큐(Circular Queue)

<img src="https://user-images.githubusercontent.com/46255148/103147472-f3740480-4798-11eb-91ea-1f451a3390e2.png" height="100px" width="280px"> ->
<img src="https://user-images.githubusercontent.com/46255148/103147480-1b636800-4799-11eb-9f5c-0f72094a14e9.png" height="100px" width="280px"> ->
<img src="https://user-images.githubusercontent.com/46255148/103147489-303ffb80-4799-11eb-8fb3-1dc8977393e4.png" height="100px" width="280px"> ->
<img src="https://user-images.githubusercontent.com/46255148/103147501-4483f880-4799-11eb-907d-f32c2cb80b3a.png" height="100px" width="280px">

-   보통 큐의 길이를 미리 정해놓고 쓰는 경우가 일반적이다. 자원이 무한하지 않으니까
-   선형 배열의 한쪽 끝과 다른 쪽 끝이 서로 맞닿아 있는 모습으로 생각하고, 큐의 맨 앞과 맨 뒤를 가리키는 인덱스를 기억해 두면 데이터 원소가 빠져 나간 쪽의 저장소를 재활용하면서 큐를 관리할 수 있게 된다. -> 이게 바로 환형 큐
-   큐의 길이를 초과하여 넣을 수 없도록 front와 rear이라는 포인터를 설정하고 enqueue, dequeue 작업을 실행한다.

## 구현

```python
class CircularQueue:
    def __init__(self, n):
        self.maxCount = n
        self.data = [None]*n
        self.count = 0  # 현재 큐에 들어있는 원소의 개수
        self.front = -1  # 데이터를 집어넣는 곳의 포인터
        self.rear = -1  # 데이터를 빼내는 곳의 포인터

    def size(self):
        return self.count

    def isEmpty(self):
        return self.count == 0

    def isFull(self):
        return self.count == self.maxCount

    def enqueue(self, x):
        if self.isFull():
            raise IndexError('Queue full')
        self.rear = (self.rear + 1) % self.maxCount
        self.data[self.rear] = x
        self.count += 1

    def dequeue(self):
        if self.isEmpty():
            raise IndexError('Queue empty')
        self.front = (self.front + 1) % self.maxCount

        # 디큐연산에선 배열에서 해당원소를 지우는 것이 아니다.
        # 대신 front포인터가 가리키는 것을 바꾸는 것이다.
        x = self.data[self.front]
        self.count -= 1
        return x

    def peek(self):
        if self.isEmpty():
            raise IndexError('Queue empty')
        return self.data[(self.front+1) % self.maxCount]
```
