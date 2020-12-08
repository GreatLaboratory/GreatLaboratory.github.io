---
title: "알고리즘 - 자료구조(4)"
excerpt: "Stack 개념 및 연습문제"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2020-12-06
---

## 스택(Stack)

-   후입선출 (Last in First out)
-   마치 접시를 차곡차곡 쌓았다가 맨 위의 접시부터 다시 꺼내어 사용하는 것처럼 추가된 데이터 원소들을 끄집어내면 마지막에 넣었던 것부터 넣은 순서의 역순으로 꺼내지는 자료 구조
-   원소를 추가하는건 push
    -   꽉 찬 스택에서 원소를 추가하려 하면?? -> 스택 오버플로우 오류 발생
-   원소를 빼내는건 pop
    -   비어있는 스택에서 원소를 빼내려 하면?? -> 스택 언더플로우 오류 발생
-   pythonds.basic.stack에서 Stack을 import해와서 사용할 수도 있다. isEmpty, items, peek, pop, push, size 등의 메소드를 사용할 수 있다.

## 구현

1.  파이썬에 내장되어 있는 Array List 자료구조인 리스트를 활용

    ```python
    class ArrayStack:
        def __init__(self):
            self.data = []

        def size(self):
            return len(self.data)

        def isEmpty(self):
            return len(self.data) == 0

        def push(self, item):
            self.data.append(item)

        def pop(self):
            return self.data.pop()

        def peek(self):
            return self.data[-1]
    ```

2.  Doubly Linked List 자료구조 활용

    ```python
    class LinkedListStack:
        def __init__(self):
            self.data = DoublyLinkedList()

        def size(self):
            return self.data.getLength()

        def isEmpty(self):
            return self.size() == 0

        def push(self, item):
            newNode = Node(item)
            self.data.insertAt(self.size()+1, newNode)

        def pop(self):
            return self.data.popAt(self.size())

        def peek(self):
            return self.data.getAt(self.size()).data

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

## 연습문제(1) - 수식의 괄호 유효성 검사

1. 여는 괄호 를 만나면 스택에 push
2. 닫는 괄호를 만났는데
3. 스택이 비어있으면 괄호 유효성 검사 실패
4. 스택에서 pop을 한 후
5. 그 원소가 닫는 괄호랑 쌍을 이루는지 검사
6. 쌍을 이루지 않으면 괄호 유효성 검사 실패
7. 만약 끝까지 검사를 마치고 스택이 비어 있다면 올바른 수식임으로 판단

```python
def solution(expr):
    match = {
        ')': '(',
        '}': '{',
        ']': '['
    }
    S = ArrayStack()
    for c in expr:
        if c in '({[':  # 1
            S.push(c)
        elif c in match:  # 2
            if S.isEmpty():  # 3
                return False
            else:
                t = S.pop()  # 4
                if t != match.get(c):  # 5
                    return False  # 6
    return S.isEmpty()  # 7


print(solution("[(A + B) * C]"))  # True
print(solution("[(A + B) * C]}"))  # False
print(solution("[(A + B) * C]{"))  # False
print(solution("{(A + B} * C)"))  # False
```

## 연습문제(2) - 수식의 중위 표기법을 후위 표기법으로 변환

1. 연산자가 아닌 알파벳의 경우 answer에 넣는다.
2. 스택이 비어있거나 '('일 경우 일단 연산자를 스택에 넣는다.
3. 괄호 연산을 닫으면 여는 괄호가 보일 때까지 스택에 쌓였던 연산자들을 전부 answer로 넣는다.
4. 스택에 넣으려는 연산자가 기존 스택에서 가장 위에 쌓인 연산자보다 우선순위가 낮으면
5. 기존 스택에 있던 모든 연산자를 꺼내어 answer에 넣고
6. 넣으려 했던 연산자를 빈 스택에 넣는다.
7. 스택에 넣으려는 연산자가 기존 스택에서 가장 위에 쌓인 연산자보다 우선순위가 높으면 스택에 쌓는다.
8. 문자열 순회가 끝난 후 스택에 남아있는 연산자들을 pop으로 전부 꺼내서 answer에 넣는다.

```python
def solution2(expr):
    answer = ''
    prec = {
        '*': 3, '/': 3,
        '+': 2, '-': 2,
        '(': 1, ')': 1,
    }
    opStack = ArrayStack()
    for s in expr:
        if s not in prec:  # 1
            answer += s
        elif opStack.isEmpty() or s == '(':  # 2
            opStack.push(s)
        elif s == ')': # 3
            popItem = opStack.pop()
            while popItem != '(':
                answer += popItem
                popItem = opStack.pop()
        elif prec.get(opStack.peek()) >= prec.get(s):  # 4
            while not opStack.isEmpty():  # 5
                answer += opStack.pop()
            opStack.push(s)  # 6
        elif prec.get(opStack.peek()) < prec.get(s):  # 7
            opStack.push(s)
    while not opStack.isEmpty():  # 8
        answer += opStack.pop()
    return answer


print(solution2('A*B+C'))  # AB*C+
print(solution2('A+B*C'))  # ABC*+
print(solution2('A+B+C'))  # AB+C+
print(solution2('A+B*C-D'))  # ABC*+D-
print(solution2('(A+B)*C'))  # AB+C*
print(solution2('C*(A+B)'))  # CAB+*
print(solution2('(A+B)*(C+D)'))  # AB+CD+*
print(solution2('(A+(B-C))*D'))  # ABC-+D*
print(solution2('A*(B-(C+D))'))  # ABCD+-*
```

## 연습문제(3) - 후위 표기 수식 계산

-   splitTokens('123 + 23 \* 3') : 문자열을 피연산자인 숫자와 연산자인 기호로 분리하여 리스트에 저장 후 리턴
-   infixToPostfix([123, '+', 23, '*', 3]) : 리스트의 순서를 후위 표기법으로 변환 후 리턴
-   postfixEval([123, 23, 4, '+', '*']) : 연산 후 결과값 리턴

```python
def splitTokens(exprStr):
    tokens = []
    val = 0
    valProcessing = False
    for c in exprStr:
        if c == ' ':
            continue
        if c in '0123456789':
            val = val * 10 + int(c)
            valProcessing = True
        else:
            if valProcessing:
                tokens.append(val)
                val = 0
            valProcessing = False
            tokens.append(c)
    if valProcessing:
        tokens.append(val)

    return tokens


def infixToPostfix(tokenList):
    prec = {
        '*': 3, '/': 3,
        '+': 2, '-': 2,
        '(': 1, ')': 1,
    }

    opStack = ArrayStack()
    postfixList = []
    for s in tokenList:
        if s not in prec:
            postfixList.append(s)
        elif opStack.isEmpty() or s == '(':
            opStack.push(s)
        elif s == ')':
            popItem = opStack.pop()
            while popItem != '(':
                postfixList.append(popItem)
                popItem = opStack.pop()
        elif prec.get(opStack.peek()) >= prec.get(s):
            while not opStack.isEmpty():
                postfixList.append(opStack.pop())
            opStack.push(s)
        elif prec.get(opStack.peek()) < prec.get(s):
            opStack.push(s)
    while not opStack.isEmpty():
        postfixList.append(opStack.pop())
    return postfixList


def postfixEval(tokenList):
    result = ArrayStack()
    for v in tokenList:
        if v == '+':
            v1 = result.pop()
            v2 = result.pop()
            result.push(v2+v1)
        elif v == '-':
            v1 = result.pop()
            v2 = result.pop()
            result.push(v2-v1)
        elif v == '*':
            v1 = result.pop()
            v2 = result.pop()
            result.push(v2*v1)
        elif v == '/':
            v1 = result.pop()
            v2 = result.pop()
            result.push(v2/v1)
        else:
            result.push(v)
    return result.pop()


def solution3(expr):
    tokens = splitTokens(expr)
    postfix = infixToPostfix(tokens)
    print(postfix)
    val = postfixEval(postfix)
    return val


print(solution3("5 + 3"))  # 8
print(solution3("(1 + 2) * (3 + 4)"))  # 21
print(solution3("7 * (9 - (3+2))"))  # 28
```
