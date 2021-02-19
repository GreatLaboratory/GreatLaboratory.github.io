---
title: "알고리즘 - DFS/BFS"
excerpt: "그래프 탐색을 위한 대표적인 두 가지 알고리즘"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2021-02-19
---

## 재귀 알고리즘

-   자기 자신을 호출하는 알고리즘
-   종료 조건이 꼭 필요
-   재귀 함수는 내부적으로 스택 자료구조와 동일하다. 함수를 계속 호출했을 때 가장 마지막에 호출한 함수가 먼저 수행을 끝내야 앞의 함수 호출이 종료되기 때문이다.
-   재귀 함수는 수학의 점화식(특정한 함수를 자신보다 더 작은 변수에 대한 함수와의 관계표현식)을 그대로 소스코드로 옮기기 때문에 코드가 간결하다.
-   팩토리얼 함수 구현

    ```python
    # 반복적으로 구현한 팩토리얼
    def factorial_iterative(n):
        result = 1
        for i in range(1, n+1):
            result *= i
        return result

    # 재귀적으로 구현한 팩토리얼
    def factorial_recursive(n):
        if n <= 1:
            return 1
        return n * factorial_recursive(n-1)

    print(factorial_recursive(5)) # 120
    print(factorial_iterative(5)) # 120
    ```

## DFS (Depth First Search)

-   특정한 경로로 탐색하다가 특정 상황에서 최대한 깊숙이 들어가서 노드를 방문한 후, 다시 돌아가 다른 경로를 탐색하는 알고리즘
-   스택 자료구조에 기초하며 구현이 간단하다. -> 스택 자료구조에 기초하므로 재귀 함수를 이용했을 때 간결하게 구현 가능하다.
-   탐색을 수행함에 있어서 데이터의 개수가 n개라면 O(n)의 시간 복잡도를 가진다.
-   탐색 순서는 아래 규칙을 따른다.
    1. 탐색 시작 노드를 스택에 삽입하고 방문 처리를 한다.
    2. 스택의 최상단 노드에 방문하지 않은 인접 노드가 있다면 그 인접 노드를 스택에 넣고 방문처리를 한다.
    3. 스택의 최상단 노드에 방문하지 않은 인접 노드가 없다면 스택에서 최상단 노드를 꺼낸다.
    4. 2, 3번 과정을 더 이상 수행할 수 없을 때까지 반복한다.

![image](https://user-images.githubusercontent.com/46255148/106998161-66749000-67c7-11eb-845d-cfb7a699d259.png)

-   위 그래프에서 DFS로 탐색했을 경우 `1-2-7-6-8-3-4-5`의 순서이다.
-   구현 코드는 아래와 같다.

    ```python
    def dfs(graph, v, visited):
        # 현재 노드를 방문 처리
        visited[v] = True
        print(v, end=' ')

        # 현재 노드와 연결된 인접 노드들을 재귀적으로 방문
        for i in graph[v]:
            if visited[i] == False:
                dfs(graph, i, visited)


    graph = [
        [],
        [2, 3, 8],
        [1, 7],
        [1, 4, 5],
        [3, 5],
        [3, 4],
        [7],
        [2, 6, 8],
        [1, 7]
    ]
    visited = [False] * 9

    dfs(graph, 1, visited)
    # 1 2 7 6 8 3 4 5
    ```

## BFS (Breadth First Search)

-   가까운 노드부터 탐색하는 알고리즘이다.
-   큐 자료구조에 기초하여 구현이 가능하다.
-   탐색을 수행함에 있어서 데이터의 개수가 n개라면 O(n)의 시간 복잡도를 가진다. (DFS보다 시간 복잡도의 성능이 좋은 편이다.)
-   탐색 순서는 아래의 규칙을 따른다.
    1. 탐색 시작 노드를 큐에 삽입하고 방문 처리를 한다.
    2. 큐에서 노드를 꺼내 해당 노드의 인접 노드 중에서 방문하지 않은 노드를 모두 큐에 삽입하고 방문 처리를 한다.
    3. 2번 과정을 더 이상 수행할 수 없을 때까지 반복한다.
-   DFS에서 사용했던 그래프에서 BFS로 탐색했을 경우 `1-2-3-8-7-4-5-6`의 순서이다.
-   구현 코드는 아래와 같다.

    ```python
    from collections import deque


    def bfs(graph, start, visited):
        # 현재 노드를 방문 처리
        visited[start] = True

        # 방문 처리된 노드는 큐에 삽입
        queue = deque([start])

        # 큐가 빌 때까지 반복
        while queue:
            v = queue.popleft()
            print(v, end=' ')

            # 아직 방문되지 않은 인접 노드들을 큐에 삽입하고 방문 처리
            for i in graph[v]:
                if visited[i] == False:
                    queue.append(i)
                    visited[i] = True


    graph = [
        [],
        [2, 3, 8],
        [1, 7],
        [1, 4, 5],
        [3, 5],
        [3, 4],
        [7],
        [2, 6, 8],
        [1, 7]
    ]
    visited = [False] * 9

    bfs(graph, 1, visited)
    # 1 2 3 8 7 4 5 6
    ```

## 예제 - 음료수 얼려먹기

![image](https://user-images.githubusercontent.com/46255148/107001785-96269680-67cd-11eb-9fb6-40fc4f373c37.png)

-   얼음 틀을 그래프로 보고 0은 아직 방문하지 않은 노드, 1은 이미 방문된 노드로 인식
-   2차원 행렬을 인덱스 순서대로 탐색하는데 현재 방문한 인덱스의 노드가 0이면 방문시키고, 동시에 인접한 노드들도 전부 방문시킨다.
-   이 과정에서 인접한 노드들이란 인접한 노드의 인접한 노드의 인접한... 노드들 전부 방문시켜야하므로 재귀 알고리즘을 떠올린다. (DFS)
-   종료 조건은 방문하려는 노드의 인덱스가 범위를 넘어설 때와 방문하려는 노드가 이미 1로 방문된 상태일 때이다.

```python
def solution(n, m, ice):
    def dfs(x, y):
        # 현재 방문하려는 노드가 범위에 벗어났다면 탈출(종료)
        if x <= -1 or x >= n or y <= -1 or y >= m:
            return False

        # 구멍이 뚫려있다면(현재 방문한 노드가 0이면) 무조건 true를 반환함과 동시에
        if ice[x][y] == 0:
            # 해당 노드를 얼음인 1로 채우고
            ice[x][y] = 1

            # 상하좌우의 노드들을 방문하며 그 노드가 구멍이 뚫려있다면(그 노드가 0이었다면) 얼음(1)로 채운다.
            dfs(x-1, y)
            dfs(x+1, y)
            dfs(x, y-1)
            dfs(x, y+1)
            return True
        else:  # 이미 얼음(1)으로 채워져있다면 탈출(종료)
            return False

    answer = 0
    for i in range(n):
        for j in range(m):
            # 첫번째 노드인 (0, 0)을 방문했을 때 dfs(0, 0)의 수행이 끝나면 ice배열에서 (0,0), (0,1), (1,0), (1,1), (1,2)은 전부 1로 채워져 있다.
            if dfs(i, j) == True:
                answer += 1
    return answer


print(solution(4, 5, [[0, 0, 1, 1, 0],
                      [0, 0, 0, 1, 1],
                      [1, 1, 1, 1, 1],
                      [0, 0, 0, 0, 0]]))  # 3

```

## 예제 - 미로 탈출

![image](https://user-images.githubusercontent.com/46255148/107001616-48aa2980-67cd-11eb-9f69-1d2e9fc41bdc.png)

-   현재 위치에서 상하좌우로 이동 가능한 가장 가까운 곳으로 이동하며 탐색하므로 BFS 알고리즘이 적합하다.
-   현재 위치에서 상하좌우 중에 이동 가능한 노드들을 전부 큐에 넣고, 큐가 빌 때까지(이동 가능한 위치가 존재하지않으므로 미로 탈출) 이동하며 탐색을 반복한다.
-   이동 가능한 위치로 이동한다면 그 위치의 노드를 이동한 거리값으로 할당한다.

```python
from collections import deque


def solution(n, m, miro):
    # 상, 하, 좌, 우
    dx = [-1, 1, 0, 0]
    dy = [0, 0, -1, 1]

    # 이동할 수 있는 노드들을 큐 자료구조로 다룬다.
    queue = deque()

    # 첫번째로 이동할 수 있는 노드는 (0, 0)으로 고정
    queue.append((0, 0))

    # 더 이상 이동할 수 있는 노드가 존재하지 않을 때까지 이동
    while queue:
        x, y = queue.popleft()

        # 상하좌우로 가장 가까운 4개의 인접 노드들을 탐색
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]

            # 미로 찾기 공간에서 벗어난 경우 무시
            if nx <= -1 or nx >= n or ny <= -1 or ny >= m:
                continue

            # 해당 노드에 괴물이 있을 경우 무시
            if miro[nx][ny] == 0:
                continue

            # 해당 노드가 이동 가능한 노드라면
            if miro[nx][ny] == 1:
                #  이 노드를 큐에 삽입 후
                queue.append((nx, ny))

                # 현재 노드에서 1을 더하여 움직인 거리값을 해당 노드 인덱스 위치에 할당
                miro[nx][ny] = miro[x][y] + 1

    # 최종적으로 맨 오른쪽 아래의 위치에 도달하는 거리값을 리턴
    return miro[n-1][m-1]


print(solution(5, 6, [
    [1, 0, 1, 0, 1, 0],
    [1, 1, 1, 1, 1, 1],
    [0, 0, 0, 0, 0, 1],
    [1, 1, 1, 1, 1, 1],
    [1, 1, 1, 1, 1, 1]
]))  # 10
```
