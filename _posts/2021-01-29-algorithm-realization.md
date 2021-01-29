---
title: "알고리즘 - 구현"
excerpt: "머릿속에 있는 알고리즘을 소스코드로 구현"
toc: true
toc_sticky: true
categories:
    - Algorithm
last_modified_at: 2021-01-29
---

## 구현 알고리즘

1. 완전 탐색 : 모든 경우의 수를 주저 없이 다 계산하여 해결하는 방법 -> O(n)
2. 시뮬레이션 : 문제에서 제시한 알고리즘을 차례 차례 직접 수행하여 해결하는 방법

## 예제 - 왕실의 나이트

-   **문제** : 8 X 8형태의 좌표평면이 주어지고 현재 위치가 주어질때 이동가능한 경우의 수를 표현해야한다. 수평으로 두 칸 이동 후 수직으로 한 칸 이동
    수직으로 두 칸 이동 후 수평으로 한 칸 이동
    2가지 경우의 규칙을 충족하는 경우의 수를 표현해야한다.
-   **유형** : 완전 탐색

```python
def solution(a):
    answer = 8
    dic = {
        'a': 1,
        'b': 2,
        'c': 3,
        'd': 4,
        'e': 5,
        'f': 6,
        'g': 7,
        'h': 8
    }
    now = [dic[a[0]], int(a[1])]
    if now[0] - 2 <= 0 or now[1] - 1 <= 0:
        answer -= 1
    if now[0] - 2 <= 0 or now[1] + 1 >= 8:
        answer -= 1
    if now[0] + 2 >= 8 or now[1] - 1 <= 0:
        answer -= 1
    if now[0] + 2 >= 8 or now[1] + 1 >= 8:
        answer -= 1

    if now[1] - 2 <= 0 or now[0] - 1 <= 0:
        answer -= 1
    if now[1] - 2 <= 0 or now[0] + 1 >= 8:
        answer -= 1
    if now[1] + 2 >= 8 or now[0] - 1 <= 0:
        answer -= 1
    if now[1] + 2 >= 8 or now[0] + 1 >= 8:
        answer -= 1

    return answer


print(solution('a1'))  # 2
print(solution('c2'))  # 6
```

## 예제 - 개임 개발

-   **문제** : N X M의 맵에서 육지와 바다가 들어온다. 육지에서만 이동할 수 있으며 현재위치를 주었을 때 이동한 횟수를 표현해라

    1. 현재 위치에서 현재방향을 기준으로 왼쪽방향(반시계 90도)부터 차례대로 갈 곳을 정한다.
    2. 캐릭터의 왼쪽방향에 아직 가보지 않은곳이 있다면 이동하고 다시 1번부터 한다.
    3. 만약 4방향 모두 이동했거나 이동하지 못한다면 뒤로 한칸을 이동한다. 만약 뒤가 바다라면 그 자리에서 멈춘다.

-   **유형** : 시뮬레이션

```python
def solution(all, now, array):
    n, m = all[0], all[1]
    d = [[0]*m for _ in range(n)]  # 지도 전체를 0으로 표시해서 아직 전부 못가본 곳으로 취급

    x, y, direction = now[0], now[1], now[2]
    d[x][y] = 1  # 시작하는 위치는 이미 방문한 곳으로 미리 체크

    # 북, 동, 남, 서 방향 정해두기
    dx = [-1, 0, 1, 0]
    dy = [0, 1, 0, -1]

    # 카운트 시작
    count = 1
    turnTime = 0
    while True:
        direction = turnLeft(direction)
        # 임시로 왼쪽으로 바꾼 방향의 위치가 갈 수 있는 곳인지를 알아보려고 잠시 nx와 ny에 저장
        nx = x + dx[direction]
        ny = y + dy[direction]
        if d[nx][ny] == 0 and array[nx][ny] == 0:  # 가본 적도 없고 지도 상에서 육지인 곳이라면 그 곳으로 이동
            d[nx][ny] = 1
            x = nx
            y = ny
            turnTime = 0
            count += 1
            continue
        else:  # 이미 가본 적이 있거나 지도상에 바다인 곳이라면
            turnTime += 1
        if turnTime == 4:
            # 바라보는 방향을 유지한 채로 한 칸 뒤로 움직이는 위치를 nx와 ny에 임시 저장 후 이게 바다인지 육지인지를 판단
            nx = x - dx[direction]
            ny = y - dy[direction]
            if array[nx][ny] == 1:
                break
            else:
                x = nx
                y = ny
                turnTime = 0

    return count


def turnLeft(direction):
    if direction == 0:
        return 3
    return direction - 1


print(solution([4, 4], [1, 1, 0], [[1, 1, 1, 1],
                                   [1, 0, 0, 1],
                                   [1, 1, 0, 1],
                                   [1, 1, 1, 1]]))

```
