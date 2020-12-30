---
title: "알고리즘 - 자료구조(9)"
excerpt: "Binary Tree 구현"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2020-12-30
---

## 이진 트리(Binary Tree)

-   모든 노드의 차수가 2 이하인 트리
-   루트 노드의 서브트리들 전부 이진 트리이며, 그 서브트리의 서브트리들 또한 전부 이진 트리이기 때문에 재귀적 성질을 가지고 있다.
-   size() : 현재 트리에 포함되어 있는 노드의 수를 구함
-   depth() : 현재 트리의 깊이를 구함
-   **traverse()** : 트리의 각 노드들을 정해진 순서에 따라 순회

    -   깊이 우선 순회(depth first traversal)

        -   중위 순회(in-order traversal) : (1) left subtree -> (2) 자기자신 -> (3) right subtree

            <img src="https://user-images.githubusercontent.com/46255148/103190845-03fdb980-4916-11eb-85fe-ff871db74875.png" height="100px" width="400px">

        -   전위 순회(pre-order traversal) : (1) 자기자신 -> (2) left subtree -> (3) right subtree

            <img src="https://user-images.githubusercontent.com/46255148/103190967-6bb40480-4916-11eb-9735-667ae3980f97.png" height="100px" width="400px">

        -   후위 순회(post-order traversal) : (1) left subtree -> (2) right subtree -> (3) 자기자신

            <img src="https://user-images.githubusercontent.com/46255148/103190986-825a5b80-4916-11eb-9310-e8792091e6b9.png" height="100px" width="400px">

    -   넓이 우선 순회(breadth first traversal)

        <img src="https://user-images.githubusercontent.com/46255148/103191065-b33a9080-4916-11eb-92e2-eeeab4293e95.png" height="100px" width="400px">

## 구현

```python
class Node:
    def __init__(self, item):
        self.data = item
        self.left = None
        self.right = None

    def size(self):  # 자신이 루트노드인 서브트리의 사이즈
        leftSize = self.left.size() if self.left else 0
        rightSize = self.right.size() if self.right else 0
        return leftSize + rightSize + 1

    def depth(self):  # 자신이 루트노드인 서브트리의 깊이
        leftDepth = self.left.depth() if self.left else 0
        rightDepth = self.right.depth() if self.right else 0
        return leftDepth + 1 if leftDepth > rightDepth else rightDepth + 1

    def inOrder(self):  # 자신이 루트노드인 서브트리의 중위 순회
        traversal = []
        if self.left:
            traversal += self.left.inOrder()
        traversal.append(self.data)
        if self.right:
            traversal += self.right.inOrder()
        return traversal

    def preOrder(self):  # 자신이 루트노드인 서브트리의 전위 순회
        traversal = []
        traversal.append(self.data)
        if self.left:
            traversal += self.left.preOrder()
        if self.right:
            traversal += self.right.preOrder()
        return traversal

    def postOrder(self):  # 자신이 루트노드인 서브트리의 후위 순회
        traversal = []
        if self.left:
            traversal += self.left.postOrder()
        if self.right:
            traversal += self.right.postOrder()
        traversal.append(self.data)
        return traversal


class BinaryTree:
    def __init__(self, r):
        self.root = r

    def size(self):
        return self.root.size() if self.root else 0

    def depth(self):
        return self.root.depth() if self.root else 0

    def inOrder(self):
        return self.root.inOrder() if self.root else []

    def preOrder(self):
        return self.root.preOrder() if self.root else []

    def postOrder(self):
        return self.root.postOrder() if self.root else []

    def breadthFirstOrder(self):  # 큐 활용
        if self.root == None:
            return []
        queue = ArrayQueue()
        queue.enqueue(self.root)  # Node가 들어옴
        traversal = []
        while queue.size() != 0:
            node = queue.dequeue()
            traversal.append(node.data)
            if node.left:
                queue.enqueue(node.left)
            if node.right:
                queue.enqueue(node.right)
        return traversal


class ArrayQueue:

    def __init__(self):
        self.data = []

    def size(self):
        return len(self.data)

    def isEmpty(self):
        return self.size() == 0

    def enqueue(self, item):
        self.data.append(item)

    def dequeue(self):
        return self.data.pop(0)

    def peek(self):
        return self.data[0]
```
