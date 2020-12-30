---
title: "알고리즘 - 자료구조(10)"
excerpt: "Binary Search Tree"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2020-12-31
---

## 이진 탐색 트리(Binary Search Tree)

![image](https://user-images.githubusercontent.com/46255148/103262263-cec19c00-49e7-11eb-8698-0699bdd822f8.png)

-   이진 탐색과 매우 비슷하다. 이진 탐색은 미리 정렬된 선형 배열에서 배열을 절반씩 잘라 가면서 찾으려는 값의 범위를 좁히는 방식이었다.
-   이진 탐색 트리는 이와 비슷하게 모든 노드에 대해서 왼쪽 서브트리에 들어 있는 노드들은 현재 탐색중인 노드의 값보다 무조건 작고,
-   오른쪽 서브트리에 들어있는 노드들은 현재 탐색중인 노드의 값보다 무조건 크며,
-   중복되는 원소는 존재하지 않는 성질을 만족하는 이진 트리이다.
-   이러한 알고리즘은 또한 재귀적인 성격을 가진다.
-   이진 탐색 트리에선 선형 배열과는 다르게 데이터의 원소를 추가하거나 삭제하는데에 강점을 보인다. 단점은 트리가 공간을 더 많이 차지한다는 것이다.
-   평균적으로 lookup탐색, insert삽입, remove삭제 연산은 O(logn)의 시간 복잡도를 가진다.
-   하지만 이진 탐색 트리가 양쪽 균형을 이루지 못하고 극단적으로 한쪽으로만 나열되어 있는 경우엔 선형배열과 다를바 없이 O(n)의 시간 복잡도를 갖게 된다.
-   remove연산은 리프 노드가 아닌 이상 트리의 구조를 재조정해야하므로 구현이 까다롭다.
    1. 삭제되는 노드가 리프 노드인 경우
    2. 삭제되는 노드가 자식을 하나 가지고 있는 경우
    3. 삭제되는 노드가 자식을 두개 가지고 있는 경우

## 구현

```python
class Node:
    def __init__(self, key, data):
        self.key = key
        self.data = data
        self.left = None
        self.right = None

    def inOrder(self):  # 자신이 루트노드인 서브트리의 중위 순회
        traversal = []
        if self.left:
            traversal += self.left.inOrder()
        traversal.append(self)
        if self.right:
            traversal += self.right.inOrder()
        return traversal

    def min(self):
        return self.left.min() if self.left else self

    def max(self):
        return self.right.min() if self.right else self

    def lookup(self, key, parent=None):
        if key < self.key:
            if self.left:
                return self.left.lookup(key, self)
            else:
                return None, None
        elif key > self.key:
            if self.right:
                return self.right.lookup(key, self)
            else:
                return None, None
        else:
            return self, parent

    def insert(self, key, data):
        if key < self.key:
            if self.left:
                self.left.insert(key, data)
            else:
                self.left = Node(key, data)
        elif key > self.key:
            if self.right:
                self.right.insert(key, data)
            else:
                self.right = Node(key, data)
        else:
            raise KeyError('중복된 원소는 삽입할 수 없습니다.')

    def countChildren(self):
        count = 0
        if self.left:
            count += 1
        if self.right:
            count += 1
        return count


class BinaryTree:
    def __init__(self):
        self.root = None

    def inOrder(self):
        return self.root.inOrder() if self.root else []

    def min(self):
        return self.root.min() if self.root else None

    def max(self):
        return self.root.max() if self.root else None

    def lookup(self, key):  # 탐색 : key를 인자로 받아서 key에 해당하는 노드와 부모노드를 리턴
        return self.root.lookup(key) if self.root else None, None

    def insert(self, key, data):
        if self.root:
            self.root.insert(key, data)
        else:
            self.root = Node(key, data)

    def remove(self, key):
        removeNode, removeParentNode = self.lookup(key)
        if removeNode == None:
            return False
        countChildren = removeNode.countChildren()
        if countChildren == 0:  # 삭제하려는 노드의 자식이 없을 경우
            if removeParentNode == None:  # 삭제하려는 노드가 root일 경우
                self.root = None
            elif removeParentNode.left == removeNode:  # 삭제하려는 노드가 부모의 left일 경우
                removeParentNode.left = None
            else:  # 삭제하려는 노드가 부모의 right일 경우
                removeParentNode.right = None
        elif countChildren == 1:  # 삭제하려는 노드의 자식이 1개일 경우
            if removeParentNode == None:  # 삭제하려는 노드가 root일 경우
                if removeNode.left:
                    self.root = removeNode.left
                else:
                    self.root = removeNode.right
            elif removeParentNode.left == removeNode:
                if removeNode.left:
                    removeParentNode.left = removeNode.left
                else:
                    removeParentNode.left = removeNode.right
            elif removeParentNode.right == removeNode:
                if removeNode.left:
                    removeParentNode.right = removeNode.left
                else:
                    removeParentNode.right = removeNode.right
        elif countChildren == 2:  # 삭제하려는 노드의 자식이 2개일 경우
            removeParentNode = removeNode
            successorNode = removeNode.right

            while successorNode.left:
                removeParentNode = successorNode
                successorNode = successorNode.left

            removeNode.key = successorNode.key
            removeNode.data = successorNode.data

            if removeParentNode.left == successorNode:
                removeParentNode.left = successorNode.right
            elif removeParentNode.right == successorNode:
                removeParentNode.right = successorNode.right
        return True
```
